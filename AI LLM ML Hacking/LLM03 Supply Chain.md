LLM03: Supply Chain

LLM03:2025 Supply Chain - THREATS Supply Chain refers to vulnerabilities in the development and deployment processes of Large Language Models (LLMs), where compromised thirdparty components—such as pre-trained models, datasets, or plugins—can introduce security risks like backdoors, biases, or system failures, potentially leading to unauthorized access or malicious behavior.

- Vulnerable models used
- 3rd party package vulnerabilities
- Out of date or vulnerable software, libraries or repositories
- Plugin exploits
- Poisoned PyPi package

LLM03:2025 Supply Chain - EXAMPLE

LLM03:2025 Supply Chain - REMEDIATION

- Vendor Security Assessment & Audits (regular audits of 3P)
- Secure Model & Data Provenance (analysis of the whole supply chain)
- Dependency Management & SBOM (Software Bill of Materials)
- Zero Trust & Access Controls
- Resilience & Redundancy Planning (fallbacks, alternative suppliers etc.)

Supply chain vulnerabilities occur when attackers exploit weaknesses in the third-party components, software, or services that an AI system depends on, potentially introducing malicious elements or compromising the integrity and security of the entire system. Examples: Out of date or vulnerable software, libraries or repositories. LLM supply chains risk integrity due to vulnerabilities leading to biases, security breaches, or system failures. Issues arise from pre-trained models, crowdsourced data, and plugin extensions.

What enables Supply Chain Vulnerabilities? Failing to thoroughly vet and audit third-party libraries, frameworks, and services for security risks before integrating them into their systems. Not continuously monitoring and updating third-party components to ensure they remain secure and up-to-date with the latest patches and security fixes.

Example 1: A compromised dataset, which is part of the LLM's supply chain, is used for training, leading the model to unintentionally generate outputs that include malware or phishing links. Example 2: Vulnerable model used for transfer learning, poisoned crowd sourced data, tampered model or data, 3rd party package vulns. Examples: OpenGPT, 3P Package exploit, Plugin exploits, poisoned PyPi package and get developers to install, trojan / backdoor in model zoo models.

How to prevent supply chain vulnerabilities? To prevent supply chain vulnerabilities, thoroughly vet and audit all third-party components, libraries, and services for security risks before integrating them into your system. Additionally, continuously monitor and promptly update these components to ensure they remain secure and upto-date with the latest patches and security fixes.

Links: [https://www.securityweek.com/chatgpt-data-breach-confirmed-as-security-firm-warns-ofvulnerable-component-exploitation/](https://www.securityweek.com/chatgpt-data-breach-confirmed-as-security-firm-warns-ofvulnerable-component-exploitation/) 
[https://securityboulevard.com/2023/05/what-happens-when-an-ai-company-falls-victim-to-asoftware-supply-chain-vulnerability/](https://securityboulevard.com/2023/05/what-happens-when-an-ai-company-falls-victim-to-asoftware-supply-chain-vulnerability/) 
[https://platform.openai.com/docs/plugins/review](https://platform.openai.com/docs/plugins/review) 
[https://pytorch.org/blog/compromised-nightly-dependency/](https://pytorch.org/blog/compromised-nightly-dependency/) 
[https://learn.microsoft.com/en-us/security/engineering/failure-modes-in-machine-learning](https://learn.microsoft.com/en-us/security/engineering/failure-modes-in-machine-learning) [https://atlas.mitre.org/techniques/AML.T0010/](https://atlas.mitre.org/techniques/AML.T0010/) 
[https://arxiv.org/pdf/1605.07277.pdf](https://arxiv.org/pdf/1605.07277.pdf) 
[https://arxiv.org/abs/1708.06733](https://arxiv.org/abs/1708.06733) 
[https://atlas.mitre.org/studies/AML.CS0002](https://atlas.mitre.org/studies/AML.CS0002)