---
title: ACTZ - AI-Powered Fitness Training
published: 2026-01-10
description: 'I built an AI-powered fitness training app using SvelteKit that generates personalized workout plans and features an interactive guided onboarding tour.'
image: ''
tags: ['SvelteKit', 'AI', 'Fitness', 'Web App', 'Project']
category: 'Projects'
draft: false 
lang: 'en'
---

# ACTZ: AI-Powered Fitness Training

https://actz.vercel.app/

To be completely honest, I didn't build ACTZ because it's an app I needed for myself. In fact, I don't even like looking at my phone during a workout! My primary motivation was simply to learn how to build something with Generative AI. While the app itself might not be my go-to gym companion, the process of creating it taught me a lot.

## What I Learned

- **New tech and libraries:** As with any new project, building ACTZ was a great opportunity to explore new tools and libraries in the SvelteKit ecosystem.
- **Data > AI:** In many applications today, "GenAI" is just a fancy buzzword—the real key is still the data. In my case, I used AI to collect and organize exercises into static structured data. 
- **AI isn't always necessary (or reliable):** I created a system prompt to generate workout plans based on the user's profile and my static exercise data. Honestly, this step could have been entirely replaced by a standard randomized algorithm with filtering rules. 
- **The reality of LLMs:** The AI component is often the least reliable part of the system. Sometimes the output makes sense, but sometimes it doesn't. You can't trust it 100%, and you constantly have to deal with challenges like prompt injection and hallucination. 

**The ultimate takeaway:** Not every application needs AI just to ride the hype wave. Sometimes, traditional programming logic is simply the better tool for the job.