# Part 1: RAGPipelineController.java
```
package com.infy.rag.controller;

import java.util.List;
import java.util.Map;

import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import reactor.core.publisher.Flux;

import com.infy.rag.service.RAGPipelineService;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.tags.Tag;

@RestController
@RequestMapping("/api/rag/advanced")
@Tag(name = "Advanced RAG", description = "Advanced Retrieval-Augmented Generation capabilities")
public class RAGPipelineController {

    private final RAGPipelineService pipelineService;

    public RAGPipelineController(RAGPipelineService advancedRAGService) {
        this.pipelineService = advancedRAGService;
    }

    @PostMapping(value = "/ingest-pdf")
    @Operation(summary = "Load and Index PDF Documents", description = """
Loads all PDF documents from the hotel-policy folder and indexes them into the vector store.

Safe to call multiple times — skips ingestion if data already exists.

Process:
1. Checks if vector store already has data (skips if so)
2. Reads all PDF files from classpath:/hotel-policy/*.pdf
3. Extracts text content using PDF reader
4. Splits documents into chunks using TokenTextSplitter
5. Embeds and stores chunks in the vector store

Returns: Confirmation message with number of chunks ingested
""")
    public ResponseEntity<String> loadData() {
        String response = pipelineService.loadData();
        return ResponseEntity.ok(response);
    }

    @GetMapping("/status")
    @Operation(summary = "Vector Store Status", description = """
Returns whether the vector store has been loaded with hotel policy documents.

Use this endpoint to verify data is available before making queries.

If vectorStoreLoaded is false, call POST /ingest-pdf first.
""")
    public ResponseEntity<Map<String, Object>> getStatus() {
        return ResponseEntity.ok(pipelineService.getStatus());
    }

    @GetMapping(value = "/full-pipeline", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    @Operation(summary = "Complete Advanced RAG Pipeline", description = """
Comprehensive multi-stage RAG pipeline demonstrating all Spring AI capabilities:

PRE-RETRIEVAL STAGE:
1. Custom Query Transformer - Standardizes hotel terminology (case-insensitive)
2. RewriteQueryTransformer - Uses clean LLM client to rewrite queries for better semantic matching
3. MultiQueryExpander - Generates 3 query variants for comprehensive retrieval

RETRIEVAL STAGE:
4. Region-Aware Document Retriever - Applies metadata filter for India/France queries,
deduplicates chunks by document ID

POST-RETRIEVAL STAGE:
5. ContextualQueryAugmenter - Augments prompt with retrieved document context

GENERATION STAGE:
6. Answer Generation - LLM generates response with hotel policy expert system prompt

Domain: Hotel Grand Heaven Policies (India, France, General)
""")
    public Flux<String> fullPipeline(

            @Parameter(
                    description = "User question about Hotel Grand Heaven policies",
                    example = "What are the hotel check-in times in India?")
            @RequestParam String question,

            @Parameter(
                    description = "Conversation ID for chat memory",
                    example = "conv-001")
            @RequestParam(defaultValue = "default") String conversationId) {

        return pipelineService.searchWithFullPipeline(question, conversationId);
    }

    @GetMapping("/capabilities")
    @Operation(
            summary = "Advanced RAG Pipeline Capabilities",
            description = "Returns detailed information about the multi-stage RAG pipeline capabilities in this project")
    public Map<String, Object> getCapabilities() {

        return Map.of(

                "projectDescription",
                "Advanced multi-stage RAG pipeline demonstrating Spring AI capabilities for Hotel Grand Heaven policy queries",

                "pipelineStages",
                Map.of(

                        "preRetrieval",
                        List.of(
                                "Custom Query Transformer - Standardizes hotel terminology (case-insensitive replacements)",
                                "RewriteQueryTransformer - Uses clean LLM client (no system prompt pollution) to rewrite queries",
                                "MultiQueryExpander - Generates 3 query variants for comprehensive retrieval"),

                        "retrieval",
                        List.of(
                                "Region-Aware Document Retriever - Applies metadata source filter for India/France queries",
                                "Deduplication by document ID to remove repeated chunks from expanded queries"),

                        "postRetrieval",
                        List.of(
                                "ContextualQueryAugmenter - Augments prompt with retrieved document context"),

                        "generation",
                        List.of(
                                "Answer Generation - LLM generates response with hotel-domain-aware system prompt")),

                "improvements",
                List.of(
                        "Duplicate ingestion guard - safe to call /ingest-pdf multiple times",
                        "Region metadata filtering - France/India queries search only relevant PDFs",
                        "Clean ChatClient for transformers - no hotel system prompt leaked into query rewriting",
                        "Document deduplication by ID - removes repeated chunks from multi-query expansion",
                        "Case-insensitive terminology transformer - handles Check-In, CHECK-IN, check-in equally",
                        "Status endpoint - verify vector store is loaded before querying",
                        "Enhanced system prompt - hotel-domain context with region awareness"),

                "availableEndpoints",
                List.of(
                        "POST /api/rag/advanced/ingest-pdf",
                        "GET /api/rag/advanced/status",
                        "GET /api/rag/advanced/full-pipeline?question=...",
                        "GET /api/rag/advanced/capabilities"),

                "domain",
                Map.of(
                        "name", "Hotel Grand Heaven Policies",
                        "regions", List.of("India", "France", "General"),
                        "policyDocuments", List.of(
                                "Hotel_Grand_Heaven_General_Policy_Guidelines.pdf",
                                "Hotel_Grand_Heaven_India_Policy.pdf",
                                "Hotel_Grand_Heaven_France_Policy.pdf")),

                "technologyStack",
                Map.of(
                        "framework", "Spring AI 1.1.0",
                        "vectorStore", "MariaDB Vector Store",
                        "llm", "GitHub Models - openai/gpt-4.1",
                        "embeddingModel", "openai/text-embedding-3-small",
                        "documentProcessing", "PDF readers and token text splitters"));
    }
}
```

# RAGPipelineService.java – Part 2
```
package com.infy.rag.service;

import java.util.ArrayList;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.client.advisor.api.Advisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.MessageWindowChatMemory;
import org.springframework.ai.chat.model.ChatModel;

import reactor.core.publisher.Flux;

import org.springframework.ai.document.Document;
import org.springframework.ai.rag.Query;
import org.springframework.ai.rag.advisor.RetrievalAugmentationAdvisor;
import org.springframework.ai.rag.generation.augmentation.ContextualQueryAugmenter;
import org.springframework.ai.rag.preretrieval.query.expansion.MultiQueryExpander;
import org.springframework.ai.rag.preretrieval.query.transformation.QueryTransformer;
import org.springframework.ai.rag.preretrieval.query.transformation.RewriteQueryTransformer;
import org.springframework.ai.rag.retrieval.search.DocumentRetriever;
import org.springframework.ai.rag.retrieval.search.VectorStoreDocumentRetriever;

import org.springframework.ai.reader.pdf.PagePdfDocumentReader;

import org.springframework.ai.transformer.splitter.TextSplitter;
import org.springframework.ai.transformer.splitter.TokenTextSplitter;

import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.ai.vectorstore.filter.FilterExpressionBuilder;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;
import org.springframework.stereotype.Service;

import io.github.resilience4j.ratelimiter.annotation.RateLimiter;

@Service
public class RAGPipelineService {

    private static final Logger log =
            LoggerFactory.getLogger(RAGPipelineService.class);

    private final ChatClient chatClient;

    private final ChatMemory chatMemory;

    private final VectorStore vectorStore;

    private final ChatModel chatModel;

    @Value("classpath:/hotel-policy/*.pdf")
    private Resource[] pdfs;

    @Value("classpath:system-prompt.st")
    Resource systemPrompt;

    public RAGPipelineService(

            ChatClient.Builder chatClientBuilder,

            VectorStore vectorStore,

            ChatModel chatModel) {

        this.chatMemory =
                MessageWindowChatMemory
                        .builder()
                        .build();

        this.chatClient =
                chatClientBuilder

                        .defaultAdvisors(

                                MessageChatMemoryAdvisor
                                        .builder(chatMemory)
                                        .order(1)
                                        .build())

                        .defaultAdvisors(

                                SimpleLoggerAdvisor
                                        .builder()
                                        .order(10)
                                        .build())

                        .build();

        this.vectorStore = vectorStore;

        this.chatModel = chatModel;
    }

    @RateLimiter(name = "embeddingLimiter")
    public String loadData() {

        List<Document> existing =
                vectorStore.similaritySearch("hotel policy");

        if (!existing.isEmpty()) {

            log.info(
                    "Vector store already contains {} chunks. Skipping ingestion.",
                    existing.size());

            return "Vector store already has data ("
                    + existing.size()
                    + " sample chunks found). Skipping ingestion. "
                    + "To force re-ingest, set remove-existing-vector-store-table=true and restart.";
        }

        List<Document> allDocs = new ArrayList<>();

        for (Resource pdf : pdfs) {

            var reader =
                    new PagePdfDocumentReader(pdf);

            List<Document> docs = reader.get();

            for (Document d : docs) {

                d.getMetadata()
                        .put(
                                "source",
                                pdf.getFilename());

            }

            allDocs.addAll(docs);
        }

        TextSplitter textSplitter =
                new TokenTextSplitter(true);

        List<Document> chunks =
                textSplitter.apply(allDocs);

        vectorStore.add(chunks);

        log.info(
                "Ingested {} chunks from {} PDFs",
                chunks.size(),
                pdfs.length);

        return "Successfully ingested "
                + chunks.size()
                + " document chunks from "
                + pdfs.length
                + " PDFs.";
    }

    public Map<String, Object> getStatus() {

        List<Document> sample =
                vectorStore.similaritySearch(
                        "hotel policy");

        return Map.of(

                "vectorStoreLoaded",
                !sample.isEmpty(),

                "sampleChunkCount",
                sample.size(),

                "hint",
                sample.isEmpty()

                        ? "Call POST /api/rag/advanced/ingest-pdf to load documents."

                        : "Vector store is ready for queries.");
    }

    public Flux<String> searchWithFullPipeline(

            String userQuery,

            String conversationId) {

        var transformBuilder =
                ChatClient.builder(chatModel);
```


```
# RGPipelineService.java – Part 3
```
        QueryTransformer hotelTerminologyTransformer = query -> {

            String transformedText = query.text()

                    .replaceAll(
                            "(?i)check-in",
                            "check-in time arrival")

                    .replaceAll(
                            "(?i)check-out",
                            "check-out time departure")

                    .replaceAll(
                            "(?i)cancellation",
                            "cancellation policy refund")

                    .replaceAll(
                            "(?i)\\bpets?\\b",
                            "pet policy animals")

                    .replaceAll(
                            "(?i)parking",
                            "parking facilities vehicle")

                    .replaceAll(
                            "(?i)shuttle",
                            "airport shuttle transportation")

                    .replaceAll(
                            "(?i)wi-?fi",
                            "Wi-Fi internet connectivity")

                    .replaceAll(
                            "(?i)\\bindia\\b|\\bindian\\b",
                            "India (Indian policy)")

                    .replaceAll(
                            "(?i)\\bfrance\\b|\\bfrench\\b",
                            "France (French policy)");

            return Query.builder()

                    .text(transformedText)

                    .build();
        };

        RewriteQueryTransformer rewriteTransformer =
                RewriteQueryTransformer.builder()

                        .chatClientBuilder(transformBuilder)

                        .build();

        MultiQueryExpander expander =
                MultiQueryExpander.builder()

                        .chatClientBuilder(transformBuilder)

                        .numberOfQueries(3)

                        .build();

        String queryLower =
                userQuery.toLowerCase();

        final String regionSource;

        if (queryLower.contains("france")
                || queryLower.contains("french")) {

            regionSource =
                    "Hotel_Grand_Heaven_France_Policy.pdf";

        } else if (queryLower.contains("india")
                || queryLower.contains("indian")) {

            regionSource =
                    "Hotel_Grand_Heaven_India_Policy.pdf";

        } else {

            regionSource = null;
        }

        if (regionSource != null) {

            log.debug(
                    "Applying region metadata filter: source = {}",
                    regionSource);
        }

        DocumentRetriever filteredRetriever = query -> {

            log.debug(
                    "Retrieving docs for query: {}",
                    query.text());

            var retrieverBuilder =
                    VectorStoreDocumentRetriever.builder()

                            .vectorStore(vectorStore)

                            .topK(10)

                            .similarityThreshold(0.3);

            if (regionSource != null) {

                retrieverBuilder.filterExpression(

                        new FilterExpressionBuilder()

                                .eq(
                                        "source",
                                        regionSource)

                                .build());
            }

            List<Document> docs =
                    retrieverBuilder
                            .build()
                            .retrieve(query);

            log.debug(
                    "Retrieved {} docs before deduplication",
                    docs.size());
```
# RAGPipelineService.java – Part 4
```
            Map<String, Document> uniqueDocs =
                    docs.stream()

                            .filter(doc ->
                                    doc.getText() != null
                                            && !doc.getText().isBlank())

                            .collect(
                                    Collectors.toMap(

                                            doc ->

                                                    String.valueOf(

                                                            doc.getMetadata()
                                                                    .getOrDefault(
                                                                            "id",
                                                                            doc.getMetadata()
                                                                                    .getOrDefault(
                                                                                            "source",
                                                                                            doc.hashCode()))),

                                            doc -> doc,

                                            (existing, duplicate) -> existing,

                                            LinkedHashMap::new));

            List<Document> deduplicatedDocs =
                    uniqueDocs.values()

                            .stream()

                            .limit(5)

                            .toList();

            log.debug(
                    "Returning {} unique docs after deduplication",
                    deduplicatedDocs.size());

            return deduplicatedDocs;
        };

        ContextualQueryAugmenter augmenter =
                ContextualQueryAugmenter.builder()

                        .allowEmptyContext(false)

                        .build();

        Advisor ragAdvisor =
                RetrievalAugmentationAdvisor.builder()

                        .queryTransformers(

                                hotelTerminologyTransformer,
                                rewriteTransformer)

                        .queryExpander(expander)

                        .documentRetriever(filteredRetriever)

                        .queryAugmenter(augmenter)

                        .build();

        return chatClient

                .prompt()

                .system(systemPrompt)

                .user(userQuery)

                .advisors(

                        MessageChatMemoryAdvisor.builder(chatMemory)

                                .conversationId(conversationId)

                                .build(),

                        ragAdvisor)

                .stream()

                .content();
    }
}
```

