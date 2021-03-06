# Solr syntax

Solr utilities for pretty print and linting. Zero dependencies

### Features

- [x] Pretty print code with nested brackets
- [x] Lint for mismatched brackets
- [x] Additional flags to customize pretty-print behavior

### Installation

```bash
go install ./...
```

### Usage

```bash
bracket-format -h
echo -e <LONG LINE> | bracket-format -max-length 100
```

Take solr params such as `bq` and `fq`:
```
echo '(resource_type:document AND ((fieldA:(94030d56-1662-45ad-b64b-4f9259db9f47))) AND ((fieldA:(94030d56-1662-45ad-b64b-4f9259db9f47)) AND (fieldB:((7c8e5ae4-c6b3-4c98-9936-90a23aa670b0 OR ed3e0c65-1daa-4d7f-8b8e-5b511f0d5adb OR ec64757c-8515-4fba-b079-5f6894ccc637 OR d5219ee8-fc12-439a-93b3-5bcaa0ea96ec OR 18f21bf0-9d32-45d7-8bbb-534082869282 OR c0508d6a-2a8a-4cfa-8dff-2775b975194d OR 2b78f5d2-b2bf-4166-94cd-e363b5df7f6a OR 004d1ad3-7f9f-4260-bb32-3ad977799946 OR d4a0be33-c1df-4f58-9256-9fea8dba75ce OR 45197ad1-2502-4f64-9647-c0b40ba71c05 OR d97cbc18-9c73-4283-ad61-af4246df68f0 OR e5c7e1ac-1022-476c-b906-66e1b953568f) AND (3069e9e5-9ea1-4359-a545-d5d25dcd574e OR 6de09b54-baef-4225-b0a7-470f95b1c2a0))) AND (fieldC:("Belgium"))))' | bracket-format
(
  resource_type:document AND
  (
    (
      fieldA:
      (
        94030d56-1662-45ad-b64b-4f9259db9f47
      )
    )
  )
  AND
  (
    (
      fieldA:
      (
        94030d56-1662-45ad-b64b-4f9259db9f47
      )
    )
    AND
    (
      fieldB:
      (
        (
          7c8e5ae4-c6b3-4c98-9936-90a23aa670b0 OR ed3e0c65-1daa-4d7f-8b8e-5b511f0d5adb OR ec64757c-8515-4fba-b079-5f6894ccc637 OR d5219ee8-fc12-439a-93b3-5bcaa0ea96ec OR 18f21bf0-9d32-45d7-8bbb-534082869282 OR c0508d6a-2a8a-4cfa-8dff-2775b975194d OR 2b78f5d2-b2bf-4166-94cd-e363b5df7f6a OR 004d1ad3-7f9f-4260-bb32-3ad977799946 OR d4a0be33-c1df-4f58-9256-9fea8dba75ce OR 45197ad1-2502-4f64-9647-c0b40ba71c05 OR d97cbc18-9c73-4283-ad61-af4246df68f0 OR e5c7e1ac-1022-476c-b906-66e1b953568f
        )
        AND
        (
          3069e9e5-9ea1-4359-a545-d5d25dcd574e OR 6de09b54-baef-4225-b0a7-470f95b1c2a0
        )
      )
    )
    AND
    (
      fieldC:
      (
        "Belgium"
      )
    )
  )
)
```

Another examples from `parsed_query`, when `debugQuery` is enabled
```bash
echo -e "+DisjunctionMaxQuery(((spanNear([textExact:query, spanOr([textExact:synonym_words, spanNear([textExact:synonymous, textExact:alias], 0, true), textExact:synonym_word])], 0, true))^100.0 | (spanNear([tag:query, spanOr([tag:synonym_words, spanNear([tag:synonymous, tag:alias], 0, true), tag:synonym_word])], 0, true))^100.0 | (spanNear([text:query, spanOr([text:synony, spanNear([text:synony, text:alias], 0, true), text:synony])], 0, true))^50.0))" | bracket-format
+DisjunctionMaxQuery
(
  (
    (
      spanNear
      (
        [
          textExact:query, spanOr
          (
            [
              textExact:synonym_words, spanNear
              (
                [
                  textExact:synonymous, textExact:alias
                ]
                , 0, true
              )
              , textExact:synonym_word
            ]
          )
        ]
        , 0, true
      )
    )
    ^100.0 |
    (
      spanNear
      (
        [
          tag:query, spanOr
          (
            [
              tag:synonym_words, spanNear
              (
                [
                  tag:synonymous, tag:alias
                ]
                , 0, true
              )
              , tag:synonym_word
            ]
          )
        ]
        , 0, true
      )
    )
    ^100.0 |
    (
      spanNear
      (
        [
          text:query, spanOr
          (
            [
              text:synony, spanNear
              (
                [
                  text:synony, text:alias
                ]
                , 0, true
              )
              , text:synony
            ]
          )
        ]
        , 0, true
      )
    )
    ^50.0
  )
)
```