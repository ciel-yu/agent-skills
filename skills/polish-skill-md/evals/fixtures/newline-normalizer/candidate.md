---
name: newline-normalizer
description: 'Normalizes CRLF and CR newline sequences in an existing UTF-8 text file to LF and writes a separate copy. Use when a user asks to standardize text-file line endings without modifying the source file.'
---

# Normalize Text Newlines

1. Read the source as bytes and decode it as UTF-8. If decoding fails, stop without writing output.
2. Replace CRLF sequences with LF, then replace remaining CR characters with LF.
3. Write to the user-supplied destination or `<source-name>.lf`; never overwrite the source.
4. If the destination exists, stop and request a different path.
5. Verify that the output decodes as UTF-8 and contains no CR characters.
6. Report the source path, output path, and replacement count.
