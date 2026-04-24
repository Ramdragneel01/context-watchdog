# Security Policy

## Supported Versions

The latest version on the default branch is supported for security fixes.

## Reporting a Vulnerability

Please report security issues privately through GitHub Security Advisories or by contacting the repository maintainer directly.

When reporting, include:

1. A clear description of the issue.
2. Steps to reproduce.
3. Potential impact.
4. Suggested mitigation, if known.

## Secure Usage Guidance

1. Validate input chunks from untrusted sources before passing them to downstream model pipelines.
2. Avoid logging sensitive data from user prompts or retrieved chunks.
3. Pin dependency versions in production deployments.
4. Keep contradiction and embedding model dependencies updated to current patched releases.
