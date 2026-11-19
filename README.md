# DocNow API: Threat Model & Security Test Plan

This repository contains a threat model and a security test plan for the DocNow API, based on the OWASP API Security Top 10.

## Contents
- [Security Test Plan](docs/test-plan.md): 10 test cases with tools and expected outcomes
- [Threat Model Diagram](docs/threat-model.drawio): data flow diagram with trust boundaries

## Scope
Client -> API Gateway -> Service -> Database

All testing is done only in my own isolated Docker lab environment.
