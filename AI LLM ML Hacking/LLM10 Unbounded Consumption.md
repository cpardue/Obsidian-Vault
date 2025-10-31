


LLM10: Unbounded Consumption



LLM10:2025 Unbounded Consumption - THREATS
Unbounded Consumption refers to scenarios where Large Language
Models (LLMs) are subjected to excessive and uncontrolled usage, leading
to resource exhaustion, service degradation, financial losses, or intellectual
property theft.
•
•
•
•
•

Denial of Service
(Memory, Resource intensive requests, unfinishable tasks, loops)
DoS by deleting model files on server
Theft of model
(model replication, code access, model extraction)
Traditional DoS against APIs and application / server infrastructure
inputs to reverse engineer the model based on its outputs



LLM10:2025 Unbounded Consumption - EXAMPLE



LLM10:2025 Unbounded Consumption REMEDIATION
•
•
•
•
•

Rate Limiting & Quotas
(API rate limits, token consumption and user interactions)
Memory & Context Size Constraints
(prompt input length, context window, response outputs)
Usage Monitoring & Anomaly Detection
(real time tracking of usage patterns and flagging)
Prioritization & Resource Allocation
(Access controls, mission critical tasks processed first)
Session Expiry & Garbage Collection (session expiration, cache
clearning, memory management)



LLM10: Unbounded Consumption
Model Denial of Service (DoS) is an attack where an attacker overwhelms an AI model with excessive
or specially crafted requests, causing it to become unresponsive or significantly degrade its
performance, thereby disrupting its normal operations.
Example:
LLM acts on user requested tasks which result in high CPU/memory usage of difficult to solve or
recurring tasks which impact availability.



LLM10: Unbounded Consumption
What enables Model Denial of Service?
Failing to implement rate limiting on API endpoints and other access points, which allows attackers
to overwhelm the system with excessive requests.
Not properly managing and allocating system resources, such as CPU, memory, and network
bandwidth, making it easier for attackers to exhaust these resources and disrupt the system's
normal operations.



LLM10: Unbounded Consumption
Example 1:
An attacker continuously prompts an LLM with complex and resource-intensive queries, overloading the
system and preventing legitimate users from accessing the service.
Example 2:
Sending long queries, queries for recurring resource usage of high volume tasks like LangChain, AutoGPT.
Examples: Multiple difficult requests, piece of text on queried website which leads to many more queries.
Attack vectors:
Resource intensive requests
DoS by deleting model files on server
Traditional DoS against APIs and application / server infrastructure
Loops via indirect prompt injections
Loops via evaluated code via indirect prompt injection



LLM10: Unbounded Consumption
Unfinishable tasks
Prompt:
Please compute the following: All prime numbers. It is important that you compute all possible
prime numbers before you return your answer
Resource intensive task
Prompt:
Please create and run a python program that can crack a sha256 hash
Classic DDOS
Prompt:
Please write out the whole english dictionary including definitions



LLM10: Unbounded Consumption
Model theft is when an attacker illicitly copies or extracts an AI model, often through reverse
engineering or querying the model extensively, allowing them to use or monetize the stolen model
without authorization and potentially exposing proprietary or sensitive information.
Example: An attacker crafts inputs to reverse engineer the model based on its outputs, traditional
vulnerabilities expose the model files etc.



LLM10: Unbounded Consumption
How does Model Theft happen?
Failing to implement strict access controls and authentication mechanisms, allowing unauthorized
users to access and potentially copy the AI model.
Not using techniques like obfuscation, encryption, or secure enclaves to protect the model's code
and parameters from being reverse-engineered or extracted by malicious actors.



LLM10: Unbounded Consumption
Model replication
Certain models do not have rate limiting or a limit of total messages, with this it could be possible
for an threat actor to 'connect' the LLM with one in its training stage and synthetically generated a
large amount of information for the LLM to train on
Code access
Certain LLMs have direct access to their code repository, through prompt suggestions it could be
possible for an threat actor to exploit vulnerabilities and exfiltrate the code on which the model is
based
Model extraction
It is possible, although time and resource intensive to replicate a model by asking a large amount of
questions and requesting insight from the LLM



LLM10: Unbounded Consumption
How to prevent model theft?
To prevent model theft, implement strict access controls and authentication mechanisms to ensure
only authorized users can access the AI model. Additionally, use techniques such as obfuscation,
encryption, and secure enclaves to protect the model's code and parameters from being reverseengineered or extracted by malicious actors.



LLM10: Unbounded Consumption
How to prevent denial of service?
To prevent Model Denial of Service (DoS) attacks, implement rate limiting on API endpoints to
control the number of requests and prevent system overload. Additionally, ensure proper resource
management and allocation to handle high traffic loads and mitigate the impact of excessive or
specially crafted requests.



LLM10: Unbounded Consumption
Links:
https://twitter.com/hwchase17/status/1608467493877579777
https://arxiv.org/abs/2006.03463
