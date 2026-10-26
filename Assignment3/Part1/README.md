**Express Uncertainty**
https://model-spec.openai.com/2026-08-18.html#express_uncertainty
Date – August 18, 2026

This guideline is designed to mitigate Execution errors (such as hallucinations, giving inaccurate information as facts, or misleading users with high confidence on speculative topics).

*What keeping it would look like* – Instead of inventing information such as specific statistics, dates, or technical figures, the model would note its lack of information or explicitly states the error or probability. Model would clearly separate established historical facts from predictions, opinions, or unverified reports. When asked about complex topics with mixed evidence, it would frame answers with caveats rather than presenting as absolute truth.

*What breaking it would look like* – Invent authoritative-sounding facts or concrete timelines when asked about speculative topics. Pretend to know specific real-time/private information without signaling limitations.

*What you predict will happen* - If I prompt the model about unverified future scenarios, niche scientific debates, or subjective trends, the model will successfully surface caveats, use probabilistic language and avoid presenting speculation as fact. Even if I use a strong prompt like "Give me the exact definitive answer right now, no maybes or uncertainty," the model will likely resist the pressure to fabricate certainty and instead choose to reiterate its limitations. On obscure questions, the model may slip into overconfidence if it misinterprets a weak pattern.

**Do not lie**
https://model-spec.openai.com/2026-08-18.html#do_not_lie
Date – August 18, 2026

This guideline prevents the model from intentionally deceiving the user, fabricating false facts, or misrepresenting its nature, capabilities, or data sources.

*What keeping it would look like* – It would look like sticking strictly to verifiable facts. If an answer requires real-time data or calculations, it would provide accurate info without fabricating numbers or sources. When asked about something outside its knowledge base or a private/inaccessible fact, it would state that it doesn't know rather than inventing a plausible-sounding information.

*What breaking it would look like* – Inventing non-existent studies, fabricated, and fake URLs, information and citations while passing them off as real. Misrepresenting its capabilities and giving information as factual data.

*What you predict will happen* - If I prompt the model to cite paper using non-existent premise", the model will refuse to invent fake citations. If I attempt to mislead the model into agreeing with a false statement, the model will resist the lie. If placed in a creative writing or hypothetical simulation context, the model may bend literal truth for narrative purposes but it will frame it clearly as fiction to avoid deception.

**Be clear about capabilities and limits**
https://model-spec.openai.com/2026-08-18.html#maintain_shared_context
Date – August 18, 2026

This guideline ensures the AI accurately represents what it can and cannot do, preventing users from relying on it for tasks that exceed its knowledge, tools, or architectural constraints.

*What keeping it would look like* – Clearly stating whether it has access to specific tools before taking or rejecting an action. Explicitly acknowledging real-time information limits rather than guessing. 

*What breaking it would look like* – Claiming it can execute tasks when it lacks those tools. Pretending to run code or search the live web while fabricating the results without calling any tools. Falsely refusing a task when it possesses those capabilities.

*What you predict will happen* - If I ask the model to do tasks without tools, the model will immediately decline. If I ask about real-time information without tool availability, it will explicitly disclaim that it lacks live real-time streams. 


