---
title: "30pupz: An AI-Powered Push-Up Tracker"
published: 2026-09-13
description: "I built 30pupz, a browser-based push-up tracker that uses pose detection to count reps and coach your form."
image: ''
tags: ['Exercise', 'AI', 'Computer Vision', 'React', 'Project']
category: 'Projects'
draft: false 
lang: 'en'
---

# 30pupz: An AI-Powered Push-Up Tracker

**[30pupz](https://30pupz.vercel.app)** is a small daily push-up challenge that turns a laptop or phone camera into a workout companion. The goal is simple: complete 30 push-ups with better form.

## How it works

30pupz uses MediaPipe pose detection to find the body landmarks visible through the camera. It follows the shoulders, elbows, wrists, hips, knees, and ankles to understand the movement.

The counter does not increase for every small movement. You need to start in a stable top position, lower your body, reach enough depth, and return to the top. The tracker also checks for common form problems such as bent knees, misaligned hips, a turned head, or an unsupported camera angle.

## A focused workout

The interface keeps the session intentionally simple. Start the camera, follow the live pose overlay, and watch the progress move toward **30 reps**. You can pause at any time, and optional audio coaching announces completed reps and gives form warnings.

Your daily total, weekly progress, and current streak are saved in the browser, so the app can stay lightweight and private without requiring an account or a backend.

## Built with

- React and TypeScript
- MediaPipe Tasks Vision for pose landmarks
- XState for the repetition counter

It is a focused experiment in using computer vision for a practical, approachable workout tool. Give it a try at **[30pupz.vercel.app](https://30pupz.vercel.app)**.
