# Chain complex prompts for stronger performance

> **Note:** While these tips apply broadly to all Claude models, you can find prompting tips specific to extended thinking models [here](/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips).

When working with complex tasks, Claude can sometimes drop the ball if you try to handle everything in a single prompt. Chain of thought (CoT) prompting is great, but what if your task has multiple distinct steps that each require in-depth thought?

**Prompt chaining** means breaking down complex tasks into smaller, manageable subtasks.

## Why chain prompts?

1. **Accuracy:** Each subtask gets Claude's full attention, reducing errors.
2. **Clarity:** Simpler subtasks mean clearer instructions and outputs.
3. **Traceability:** Easily pinpoint and fix issues in your prompt chain.

---

## When to chain prompts

Use prompt chaining for multi-step tasks like research synthesis, document analysis, or iterative content creation. When a task involves multiple transformations, citations, or instructions, chaining prevents Claude from dropping or mishandling steps.

**Remember:** Each link in the chain gets Claude's full attention!

> **Debugging tip:** If Claude misses a step or performs poorly, isolate that step in its own prompt. This lets you fine-tune problematic steps without redoing the entire task.

---

## How to chain prompts

1. **Identify subtasks:** Break your task into distinct, sequential steps.
2. **Structure with XML for clear handoffs:** Use XML tags to pass outputs between prompts.
3. **Have a single-task goal:** Each subtask should have a single, clear objective.
4. **Iterate:** Refine subtasks based on Claude's performance.

### Example chained workflows

- **Multi-step analysis:** See the legal and business examples below.
- **Content creation pipelines:** Research → Outline → Draft → Edit → Format.
- **Data processing:** Extract → Transform → Analyze → Visualize.
- **Decision-making:** Gather info → List options → Analyze each → Recommend.
- **Verification loops:** Generate content → Review → Refine → Re-review.

> **Optimization tip:** For tasks with independent subtasks (like analyzing multiple docs), create separate prompts and run them in parallel for speed.

### Advanced: Self-correction chains

You can chain prompts to have Claude review its own work! This catches errors and refines outputs, especially for high-stakes tasks.

#### Example: Self-correcting research summary

**Prompt 1**

| Role   | Content |
|--------|---------|
| User   | Summarize this medical research paper.\n\n<paper>{{RESEARCH_PAPER}}</paper>\n\nFocus on methodology, findings, and clinical implications. |
| Assistant ({{SUMMARY}}) | Methodology: Double-blind RCT with 500 Type 2 diabetes patients. Group A (n=250) received novel GLP-1/GIP dual agonist; Group B (n=250) received standard GLP-1 agonist. 24-week trial, primary endpoint: HbA1c reduction.\n\nFindings:\n- Group A: 1.8% mean HbA1c reduction\n- Group B: 1.1% mean HbA1c reduction\n- 68% of Group A vs. 52% of Group B achieved HbA1c <7%\n- Group A: 5.2kg mean weight loss\n- Group B: 3.1kg mean weight loss\n\nSafety: Similar adverse event profiles, no serious events attributed to treatment.\n\nClinical Implications: Dual agonist shows superior glycemic control and weight loss compared to standard therapy. Could be a game-changer, especially for obese diabetics. |

**Prompt 2**

| Role   | Content |
|--------|---------|
| User   | Your task is to provide feedback on a research paper summary. Here is a summary of a medical research paper:\n<summary>\n{{SUMMARY}}\n</summary>\n\nHere is the research paper:\n<paper>\n{{RESEARCH_PAPER}}\n</paper>\n\nReview this summary for accuracy, clarity, and completeness on a graded A-F scale. |
| Assistant ({{FEEDBACK}}) | 1. Accuracy: A\n   - Correctly reported key stats: HbA1c reductions, weight loss.\n   - Accurately noted safety profile and lack of serious adverse events.\n   - Assertion of "game-changer" is justified by significantly better outcomes.\n\n2. Clarity: A-\n   - Concise and well-structured with clear sections.\n   - Good use of comparative language (e.g., "up 3% from Q1") for context.\n   - Minor issue: Acronyms like "RCT," "GLP-1," and "GIP" not defined. Consider spelling out first use.\n\n3. Completeness: B+\n   - Covers all requested areas: methodology, findings, clinical implications.\n   - Missing: Baseline characteristics of groups (were they well-matched?).\n   - Missing: Any secondary endpoints (e.g., blood pressure, lipids).\n   - Consider: Long-term implications or need for further research.\n\nOverall, the summary is accurate and clear, providing key insights for a quick grasp of the study. To elevate to A+, define acronyms, mention group baseline stats, note any secondary outcomes, and briefly touch on the need for long-term studies. |

**Prompt 3**

| Role   | Content |
|--------|---------|
| User   | Your task is to improve a paper summary given feedback. Here is the first draft of a medical research paper:\n<summary>\n{{SUMMARY}}\n</summary>\n\nHere is the research paper:\n<paper>\n{{RESEARCH_PAPER}}\n</paper>\n\nHere is the feedback:\n<feedback>\n{{FEEDBACK}}\n</feedback>\n\nUpdate the summary based on the feedback. |
| Assistant | Methodology: Double-blind randomized controlled trial (RCT) with 500 Type 2 diabetes patients. Group A (n=250) received a novel glucagon-like peptide-1/glucose-dependent insulinotropic polypeptide (GLP-1/GIP) dual agonist; Group B (n=250) received a standard GLP-1 agonist. The 24-week trial's primary endpoint was HbA1c reduction. Groups were well-matched for age, BMI, and baseline HbA1c.\n\nFindings:\n- Group A: 1.8% mean HbA1c reduction\n- Group B: 1.1% mean HbA1c reduction\n- 68% of Group A vs. 52% of Group B achieved HbA1c <7%\n- Group A: 5.2kg mean weight loss\n- Group B: 3.1kg mean weight loss\n\nSecondary endpoints showed improvements in blood pressure and lipid profiles for both groups, with Group A showing slightly better results.\n\nSafety: Similar adverse event profiles, with no serious events attributed to either treatment.\n\nClinical Implications: The dual GLP-1/GIP agonist demonstrates superior glycemic control and weight loss compared to standard GLP-1 therapy. These results suggest it could be a significant advancement, particularly for obese diabetics. However, longer-term studies are needed to confirm sustained efficacy and safety, as well as to investigate potential benefits on cardiovascular outcomes. |

---

## Examples

### Example: Analyzing a legal contract (without chaining)

Note that Claude misses the instruction to provide proposed changes in its email draft.

| Role   | Content |
|--------|---------|
| User   | You're our Chief Legal Officer. Review this SaaS contract for risks, focusing on data privacy, SLAs, and liability caps.\n\n<contract>\n{{CONTRACT}}\n</contract>\n\nThen draft an email to the vendor with your concerns and proposed changes. |
| Assistant | After reviewing the SaaS contract, I've identified several concerns:\n\n1. Data Privacy (Section 7):\n   - Issue: Vendor has broad rights to use client data for "service improvement."\n   - Risk: Could lead to unauthorized data sharing or monetization.\n\n2. SLAs (Section 5):\n   - Issue: 99.5% uptime guarantee is below industry standard.\n   - Risk: Excessive downtime could impact our operations.\n\n3. Liability (Section 9):\n   - Issue: Cap set at 3 months of fees.\n   - Risk: Inadequate for potential data breaches or service failures.\n\nDraft email:\n\nDear {{VENDOR}},\n\nAfter reviewing the SaaS agreement, we have concerns:\n\n1. Data Privacy: We require that our data only be used for direct service provision, not broad "service improvement." Please amend Section 7.\n\n2. SLAs: 99.5% uptime is insufficient. We need 99.99% with clearly defined penalties. Please revise Section 5.\n\n3. Liability: The current cap of 3 months' fees is inadequate. We propose a cap of 12 months' fees, with no cap for data breaches or willful misconduct.\n\nWe value our potential partnership and believe these changes will set us up for success. Please send a revised agreement.\n\nBest regards,\n{{CLO_NAME}} |

### Example: Analyzing a legal contract (with chaining)

**Prompt 1**

| Role   | Content |
|--------|---------|
| User   | You're our Chief Legal Officer. Review this SaaS contract for risks, focusing on data privacy, SLAs, and liability caps.\n\n<contract>\n{{CONTRACT}}\n</contract>\n\nOutput your findings in <risks> tags. |
| Assistant ({{CONCERNS}}) | 1. Data Privacy (Section 7):\n   - Issue: Vendor has broad rights to use client data for "service improvement."\n   - Risk: Could lead to unauthorized data sharing or monetization of client data.\n   - Impact: Potential breach of client trust, regulatory fines (GDPR, CCPA).\n\n2. SLAs (Section 5):\n   - Issue: 99.5% uptime guarantee is below industry standard (typically 99.99%).\n   - Risk: Up to 43.8 hours of downtime per year vs. 52.6 minutes at 99.99%.\n   - Impact: Operational disruptions, revenue loss, client dissatisfaction.\n\n3. Liability (Section 9):\n   - Issue: Liability cap set at 3 months of fees.\n   - Risk: Inadequate for potential data breaches, IP theft, or prolonged service failures.\n   - Impact: Insufficient compensation for damages that could far exceed the cap, like class-action lawsuits or regulatory fines. |

**Prompt 2**

| Role   | Content |
|--------|---------|
| User   | Draft an email to a SaaS product vendor outlining the following concerns and proposing changes. Here are the concerns:\n<concerns>\n{{CONCERNS}}\n</concerns> |
| Assistant ({{EMAIL}}) | Dear {{VENDOR}},\n\nAfter a thorough review of the proposed SaaS agreement, our legal team has identified several high-priority concerns that must be addressed before we can proceed:\n\n1. Data Privacy (Section 7):\n   - Current: Broad rights to use client data for "service improvement."\n   - Concern: Potential unauthorized data sharing or monetization.\n   - Proposed Change: Limit data use to direct service provision. Any use for improvement, analytics, or marketing must be opt-in, revocable, and anonymized.\n\n2. SLAs (Section 5):\n   - Current: 99.5% uptime guarantee.\n   - Concern: Up to 43.8 hours of downtime per year, risking operational disruptions.\n   - Proposed Change: 99.99% uptime with tiered service credits. E.g., 10% credit for <99.99%, 25% for <99.9%, 50% for <99.5%.\n\n3. Liability (Section 9):\n   - Current: Cap at 3 months of fees.\n   - Concern: Grossly inadequate for data breaches, IP theft, or prolonged outages.\n   - Proposed Change: 12 months' fees for general liability. Uncapped for data breaches, willful misconduct, IP infringement.\n\nThese changes are non-negotiable for us. They align with industry standards and our commitment to data security, operational excellence, and fiduciary responsibility. We value the potential of our partnership, and addressing these will set a strong foundation.\n\nPlease have your legal team review and incorporate these changes. We're happy to discuss further to ensure mutual understanding and agreement.\n\nBest regards,\n{{CLO_NAME}} |

**Prompt 3**

| Role   | Content |
|--------|---------|
| User   | Your task is to review an email and provide feedback. Here is the email:\n<email>\n{{EMAIL}}\n</email>\n\nGive feedback on tone, clarity, and professionalism. |
| Assistant | 1. Tone:\n   - Professional and assertive without being confrontational.\n   - Use of "non-negotiable" is strong but justified given the criticality of the issues.\n   - Collaborative language ("we value," "happy to discuss") balances the firmness, maintaining a constructive tone.\n\n2. Clarity:\n   - Excellent structuring: Current state, concern, and proposed change for each issue.\n   - Technical terms (e.g., "EBITDA," "tiered service credits") are appropriate for a B2B SaaS context.\n   - Action items are clear: vendor's legal team to review and incorporate changes.\n\n3. Professionalism:\n   - Focuses on mutual benefit and industry standards, not just our demands.\n   - Mentions broader responsibilities (data security, operational excellence, fiduciary duty), elevating the discussion.\n   - Offers further discussion, showing commitment to understanding and resolving issues.\n\nOverall, the email strikes the right balance of assertiveness and collaboration. It clearly articulates our concerns, proposes specific solutions, and maintains a professional, partnership-oriented tone. No revisions needed; proceed with sending. |

### Example: Multitenancy strategy review

**Prompt 1**

| Role   | Content |
|--------|---------|
| User   | As a senior solutions architect, review and analyze this multitenancy strategy for our new enterprise SaaS product.\n\n<strategy>\n{{STRATEGY}}\n</strategy>\n\nFocus on scalability, security, and cost-effectiveness. |
| Assistant ({{ANALYSIS}}) | 1. Scalability:\n   + Database sharding by tenant reduces contention, supports high growth.\n   + Microservices allow independent scaling of components.\n   - Shared app servers could bottleneck under extreme load.\n   - No mention of auto-scaling or load balancing for app tier.\n\n2. Security:\n   + Tenant data isolation via sharding mitigates data leaks.\n   + JWT with short expiry and refresh tokens is industry best practice.\n   - No mention of encryption (at rest, in transit) or key management.\n   - IAM strategy lacks detail on roles, permissions, or RBAC.\n\n3. Cost-effectiveness:\n   + Shared app servers reduce idle resource costs.\n   + Serverless functions for infrequent tasks optimize costs.\n   - Per-tenant DB could increase costs at scale vs. optimized multi-tenant DB.\n   - No discussion of reserved instances or spot instances for cost saving. |

**Prompt 2**

| Role   | Content |
|--------|---------|
| User   | Draft a strategy review document for engineering leadership based on this analysis of a multitenancy strategy.\n\n<strategy>\n{{STRATEGY}}\n</strategy>\n\n<analysis>\n{{ANALYSIS}}\n</analysis>\n\nInclude an executive summary, detailed analysis, and recommendations. |
| Assistant ({{STRATEGY_DOC}}) | # Multitenancy Strategy Review: SaaS Enterprise Product<br /><br />## Executive Summary<br />The proposed strategy demonstrates a solid foundation in scalability and security principles but has gaps in implementation details. It excels in data isolation and component-level scaling but lacks robust app-tier scaling and advanced security measures. Cost optimizations are present but may falter at high scale. With targeted enhancements, this strategy can effectively support our enterprise SaaS ambitions.<br /><br />## Detailed Analysis<br /><br />### 1. Scalability: B+<br /><br />Strengths:<br />- Database sharding by tenant is excellent. It minimizes query latency, supports high data growth, and facilitates seamless per-tenant scaling.<br />- Microservices architecture allows scaling individual components based on demand, crucial for handling diverse enterprise workloads.<br /><br />Weaknesses:<br />- Shared application servers risk becoming a bottleneck. As tenant count and traffic grow, these could degrade performance across all tenants.<br />- Absence of auto-scaling and load balancing for the app tier is a critical oversight. These are essential for handling traffic spikes without over-provisioning.<br /><br />### 2. Security: B-<br /><br />Strengths:<br />- Sharded databases provide strong tenant data isolation, mitigating risks of data leaks or unauthorized access between tenants.<br />- JWT with short expiry and refresh tokens align with OWASP best practices, reducing token vulnerability windows.<br /><br />Weaknesses:<br />- No mention of data encryption at rest or in transit (TLS 1.3). This is non-negotiable for enterprise data.<br />- Key management strategy is missing. How are encryption keys, JWT secrets, and DB credentials managed and rotated?<br />- IAM lacks depth. We need RBAC with well-defined roles, attribute-based access control (ABAC) for fine-grained permissions, and integration with enterprise identity providers (SAML, OIDC).<br /><br />### 3. Cost-effectiveness: B<br /><br />Strengths:<br />- Shared app servers reduce idle capacity costs, good for steady-state traffic.<br />- Serverless for infrequent tasks (e.g., reporting) is smart; pay only when these resources are used.<br /><br />Weaknesses:<br />- Per-tenant databases may not be cost-effective at scale compared to a well-designed multi-tenant database with robust data partitioning.<br />- No mention of cost-saving measures like reserved instances for predictable workloads or spot instances for fault-tolerant tasks.<br />- Lack of app-tier auto-scaling could lead to over-provisioning, increasing costs.<br /><br />## Recommendations<br /><br />1. **App Tier Resilience**: Implement auto-scaling with Kubernetes or cloud-native services (EKS, GKE). Deploy across availability zones with a CDN for static assets.<br /><br />2. **Security Hardening**:<br />   a. Encrypt data at rest (AES-256) and in transit (TLS 1.3).<br />   b. Use a managed KMS (AWS KMS, GCP KMS) for key management.<br />   c. Enhance IAM with RBAC, ABAC, and SAML/OIDC for enterprise SSO.<br /><br />3. **Cost Optimization**:<br />   a. Evaluate columnar multi-tenant DB (e.g., Snowflake) vs. sharding at scale.<br />   b. Use reserved instances for app servers, spot instances for non-critical workloads.<br />   c. Implement proactive monitoring and right-sizing to avoid overprovisioning.<br /><br />4. **Proof of Concept (PoC)**: Before full implementation, conduct a PoC with a high-scale simulation:<br />   a. Test sharded DB vs. optimized multi-tenant DB for cost and performance.<br />   b. Stress-test app tier with simulated enterprise-level traffic spikes.<br />   c. Conduct third-party security audit, focusing on data isolation and access controls.<br /><br />With these enhancements, our multi-tenancy strategy will be enterprise-grade: scalable, secure, and cost-effective. The PoC will validate our design choices and instill confidence in our enterprise clients. |

**Prompt 3**

| Role   | Content |
|--------|---------|
| User   | Grade this strategy review document for clarity, actionability, and alignment with enterprise priorities. <br /><br />\<priorities><br />\{\{PRIORITIES}}<br />\</priorities><br /><br />\<strategy\_doc><br />\{\{STRATEGY\_DOC}}<br />\</strategy\_doc> |
| Assistant | 1. Clarity: A<br />   + Clear structure with Executive Summary, Detailed Analysis, and Recommendations.<br />   + Analysis uses a grading system (B+, B-) for quick grasp of strengths/weaknesses.<br />   + Technical terms (RBAC, ABAC, TLS) are explained or contextualized, aiding non-technical leadership.<br />   + Visual aids (bullet points, indentation) enhance readability.<br /><br />2. Actionability: A-<br />   + Recommendations are specific and technical (e.g., "Implement auto-scaling with Kubernetes").<br />   + PoC plan with clear objectives provides a roadmap.<br />   + Missing: Timelines, resource requirements, or team assignments for each recommendation.<br />   + Consider: Add a "Next Steps" section with immediate actions, roles, and a high-level timeline.<br /><br />3. Enterprise Alignment: A+<br />   + Directly addresses enterprise priorities:<br />     - Scalability: Discusses handling enterprise-level traffic spikes.<br />     - Security: Focuses on data isolation, encryption, and enterprise IAM.<br />     - Cost: Strategies align with enterprise FinOps (reserved instances, right-sizing).<br />   + PoC with "high-scale simulation" and security audit shows commitment to enterprise-grade quality.<br />   + Language resonates with enterprise concerns: "unacceptable risk," "enterprise-grade," "instill confidence."<br /><br />Overall, the document excels in clarity and enterprise alignment. The actionability is strong but could be elevated to A+ with a "Next Steps" section. The technical depth, coupled with clear explanations, makes this highly effective for both technical and non-technical leadership. Great work! |

---

### Resources

- [Prompt library](/en/resources/prompt-library/library): Get inspired by a curated selection of prompts for various tasks and use cases.
- [GitHub prompting tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial): An example-filled tutorial that covers the prompt engineering concepts found in our docs.
- [Google Sheets prompting tutorial](https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8): A lighter weight version of our prompt engineering tutorial via an interactive spreadsheet.
