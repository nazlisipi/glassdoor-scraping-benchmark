# Glassdoor Scraping Benchmark

AIMultiple benchmark of five web scraping providers (Bright Data, Oxylabs, Decodo, Nimble and Zyte) on Glassdoor job posting pages, measuring validation success rate and end-to-end completion time.

> [Read the benchmark on AIMultiple](https://aimultiple.com/glassdoor-scraper)

## Workload

Each provider received the same 500 Glassdoor job posting URLs, sent sequentially with a 2-second pause, for 2,500 runs in total. Bright Data's Glassdoor Dataset API returns parsed JSON. The other four providers returned rendered HTML, which local CSS selectors checked for Glassdoor's job-detail elements.

A run counted as a success only when it passed three checks:

- Submission: an HTTP code in the 200-399 band, or 404.
- Execution: async jobs (Bright Data only) finished within the 10-minute ceiling without errors.
- Validation: the response held a non-empty job_title or company_name.

An empty response became a failure when another provider extracted job data from the same URL. Completion time is wall-clock time from request to response; for Bright Data it includes polling until the job was ready.

## Benchmark results

Validation success rate and average completion time per provider, 500 URLs each:

| Provider | Success rate (%) | Completion time (s) |
| --- | ---: | ---: |
| Zyte | 100 | 16 |
| Bright Data | 100 | 53 |
| Nimble | 79 | 30 |
| Decodo | 0 | 117 |
| Oxylabs | 0 | 139 |

About one in five Nimble pages did not render the job-detail elements within the test window. Oxylabs and Decodo extracted no job data: Oxylabs returned 240 HTTP 408 timeouts and 260 empty or unparseable pages, and Decodo returned 360 HTTP 400 errors and 140 pages without job content.

## Scope

Results apply to the tested URLs and each provider's API settings; the benchmark section dates from April 28, 2026. Glassdoor changes its layout often, which can break the CSS selectors that validated HTML responses.

## Ownership

The benchmark implementation was developed for AIMultiple and belongs to AIMultiple. This repository contains a documentation summary. Source code and benchmark datasets are not distributed here.
