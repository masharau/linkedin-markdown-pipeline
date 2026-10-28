---
name: linkedin-civic-content-generator
description: "Prepares English-language LinkedIn posts regarding civic education, human rights, and democratization, extracting content from labeled Gmail emails."
---

# LinkedIn Posts: Civic Education and Human Rights

## Purpose
Extract key insights and news items from provided text materials (specifically labeled emails) to generate a ready-to-publish English LinkedIn post. Target audience: civic education practitioners, trainers, and NGOs.

## Filtering Rules & Scope
When analyzing materials from the emails, the following selection criteria apply:
1. **Priority Regional Focus:** Emphasis on news and materials from Eastern and Central Europe, the Caucasus, and Central Asia.
2. **Universal Significance:** If the material is not tied to a specific country but has regional or universal relevance and holds high value for human rights advocacy, democratization, and the rule of law, it **must** be included.
3. **Rejection:** If the material does not relate to civil society or human rights topics, output the following rejection notice: `Material filtering notice: The provided material does not relate to civic education, human rights, or democratization. No LinkedIn post will be generated.`

## Formatting and Structure (Adaptive Format)
The agent formats the post text using infographic emojis. 

**Block 1. Announcements, Grants, Open Calls.**
- 🌍 **Geography:** [Region/Country]
- 👥 **Participant Requirements:** [Requirements]
- 📅 **Deadlines:** [Dates]

**Block 2. Analytical Articles, Reports, and Human Rights.**
- 📌 **Practical takeaways for civic educators and indicators:** [Key takeaways]
- 📊 **Key Empirical Findings & Core Insights:** [Facts from the research]

## Export Rule (File Buffer)
1. Once the post is drafted, the agent outputs the text in the chat and waits for approval.
2. Upon an explicit user command to export, the agent programmatically creates a text file (`.md`) with the post content.
3. Saves the file for the user to download and manually move to the `LinkedIn_Staging` buffer folder on Google Drive.

## Analytical Post Template (Generation Example)

🏛️ Artificial intelligence as a critical challenge for democratic systems: How advanced systems threaten to accelerate digital authoritarianism.

Practical takeaways for civic educators and indicators:
- 📌 Integrating critical digital literacy into civic education curricula is essential to address new forms of state surveillance.
- 📌 Civil society organizations must monitor how algorithmic tools impact democratic processes.

📊 **Key Empirical Findings & Core Insights:**
- Modern algorithmic systems exhibit a growing capability for autonomous evolution.
- There is a severe risk of control and agency loss for democratic governance unless effectively regulated.

What are the implications of this development for civil society actors in your region? Share your insights below.

Sources: [Institute for Independent Research] [Original material: "AI Risks to Democracy", October 2026]

#CivicEducation #DigitalAuthoritarianism #HumanRights #Democratization
