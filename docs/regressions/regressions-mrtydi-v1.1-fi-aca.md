# Anserini Regressions: Mr. TyDi (v1.1) &mdash; Finnish

**Models**: bag-of-words approaches using `AutoCompositeAnalyzer`

This page documents BM25 regression experiments for [Mr. TyDi (v1.1) &mdash; Finnish](https://github.com/castorini/mr.tydi) using `AutoCompositeAnalyzer`.

The exact configurations for these regressions are stored in [this YAML file](../../src/main/resources/regression/mrtydi-v1.1-fi-aca.yaml).
Note that this page is automatically generated from [this template](../../src/main/resources/docgen/templates/mrtydi-v1.1-fi-aca.template) as part of Anserini's regression pipeline, so do not modify this page directly; modify the template instead.

From one of our Waterloo servers (e.g., `orca`), the following command will perform the complete regression, end to end:

```
python src/main/python/run_regression.py --index --verify --search --regression mrtydi-v1.1-fi-aca
```

## Indexing

Typical indexing command:

```
target/appassembler/bin/IndexCollection \
  -collection MrTyDiCollection \
  -input /path/to/mrtydi-v1.1-fi \
  -index indexes/lucene-index.mrtydi-v1.1-finnish-aca/ \
  -generator DefaultLuceneDocumentGenerator \
  -threads 1 -storePositions -storeDocvectors -storeRaw -language fi -useAutoCompositeAnalyzer \
  >& logs/log.mrtydi-v1.1-fi &
```

See [this page](https://github.com/castorini/mr.tydi) for more details about the Mr. TyDi corpus.
For additional details, see explanation of [common indexing options](../../docs/common-indexing-options.md).

## Retrieval

After indexing has completed, you should be able to perform retrieval as follows:

```
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.mrtydi-v1.1-finnish-aca/ \
  -topics tools/topics-and-qrels/topics.mrtydi-v1.1-fi.train.txt.gz \
  -topicreader TsvInt \
  -output runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.train.txt \
  -bm25 -hits 100 -language fi -useAutoCompositeAnalyzer &
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.mrtydi-v1.1-finnish-aca/ \
  -topics tools/topics-and-qrels/topics.mrtydi-v1.1-fi.dev.txt.gz \
  -topicreader TsvInt \
  -output runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.dev.txt \
  -bm25 -hits 100 -language fi -useAutoCompositeAnalyzer &
target/appassembler/bin/SearchCollection \
  -index indexes/lucene-index.mrtydi-v1.1-finnish-aca/ \
  -topics tools/topics-and-qrels/topics.mrtydi-v1.1-fi.test.txt.gz \
  -topicreader TsvInt \
  -output runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.test.txt \
  -bm25 -hits 100 -language fi -useAutoCompositeAnalyzer &
```

Evaluation can be performed using `trec_eval`:

```
tools/eval/trec_eval.9.0.4/trec_eval -c -M 100 -m recip_rank -c -m recall.100 tools/topics-and-qrels/qrels.mrtydi-v1.1-fi.train.txt runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.train.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -M 100 -m recip_rank -c -m recall.100 tools/topics-and-qrels/qrels.mrtydi-v1.1-fi.dev.txt runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.dev.txt
tools/eval/trec_eval.9.0.4/trec_eval -c -M 100 -m recip_rank -c -m recall.100 tools/topics-and-qrels/qrels.mrtydi-v1.1-fi.test.txt runs/run.mrtydi-v1.1-fi.bm25.topics.mrtydi-v1.1-fi.test.txt
```

## Effectiveness

With the above commands, you should be able to reproduce the following results:

| **MRR@100**                                                                                                  | **BM25**  |
|:-------------------------------------------------------------------------------------------------------------|-----------|
| [Mr. TyDi (Finnish): train](https://github.com/castorini/mr.tydi)                                            | 0.4176    |
| [Mr. TyDi (Finnish): dev](https://github.com/castorini/mr.tydi)                                              | 0.4193    |
| [Mr. TyDi (Finnish): test](https://github.com/castorini/mr.tydi)                                             | 0.2903    |
| **R@100**                                                                                                    | **BM25**  |
| [Mr. TyDi (Finnish): train](https://github.com/castorini/mr.tydi)                                            | 0.8346    |
| [Mr. TyDi (Finnish): dev](https://github.com/castorini/mr.tydi)                                              | 0.8458    |
| [Mr. TyDi (Finnish): test](https://github.com/castorini/mr.tydi)                                             | 0.7529    |
