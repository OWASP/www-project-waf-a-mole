---

layout: col-sidebar
title: OWASP WAF-A-MoLE
tags: example-tag
level: 2
type: code
pitch: A very brief, one-line description of your project

---

Project Overview
A guided mutation-based fuzzer for Web Application Firewalls.
Given an input attack payload, it tries to produce a semantic invariant query that bypasses detection rules for the target WAF. You can use this tool for assessing the robustness of your product by letting WAF-A-MoLE explore the solution space to find dangerous "blind spots" left uncovered by the target classifier.

Architecture
WAF-A-MoLE takes an initial payload and inserts it in the payload Pool, which manages a priority queue ordered by the WAF confidence score over each payload.

During each iteration, the head of the payload Pool is passed to the Fuzzer, where it gets randomly mutated, by applying one of the available mutation operators.
Mutation operators
Mutations operators are all semantics-preserving and they leverage the expressive power of the target grammar.

Below are the SQL mutation operators available in the current version of WAF-A-MoLE.

Mutation			Example
Case Swapping		admin' OR 1=1# ⇒ admin' oR 1=1#
Whitespace Substitution	admin' OR 1=1# ⇒ admin'\t\rOR\n1=1#
Comment Injection		admin' OR 1=1# ⇒ admin'/**/OR 1=1#
Comment Rewriting		admin'/**/OR 1=1# ⇒ admin'/*xyz*/OR 1=1#abc
Integer Encoding		admin' OR 1=1# ⇒ admin' OR 0x1=(SELECT 1)#
Operator Swapping		admin' OR 1=1# ⇒ admin' OR 1 LIKE 1#
Logical Invariant		admin' OR 1=1# ⇒ admin' OR 1=1 AND 0<1#
Number Shuffling		admin' OR 1=1# ⇒ admin' OR 2=2#

How to Contribute
Questions, bug reports and pull requests are always welcome.
In particular, if you are interested in expanding this project, we are currently interested in the following contributions:

* New WAF adapters
* New mutation operators (both for SQL and other vulnerability classes, XSS in particular)
* New search algorithms

### Road Map
March 2024: WAF-A-MoLE becomes a OWASP project
This version supports SQL mutations and contains drivers for ModSecurity and a selection of ML-based WAFs
The ModSecurity driver leverages on a custom fork for the pymodsecurity module.

March 2024 - September 2024: Testing + Feedback + Improvements
These tests will include assessing how effective the current strategy is at bypassing ModSecurity (with a focus on the CoreRuleSet), also by gathering real experiences from users.
Compare and add optimization algorithms to the WAF-A-MoLE strategy selection.

September 2024: Integrate WAF-A-MoLE in the FTW framework
FTW currently uses static payloads, we want to add dynamic payload generation.

March 2025: XSS support
WAF-A-MoLE includes mutation operators and strategies for Cross-Site Scripting payloads
