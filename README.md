# 💎 Jetpack Compose Glassmorphism AI Skill

An open-source, production-grade **AI Skill** that codifies the complete design guidelines, color tokens, and implementation patterns for the **"Trendy Glassmorphism 2.0"** design system in **Jetpack Compose** and **Compose Multiplatform (KMP)**.

This skill is designed to be loaded directly by modern AI coding assistants (like **Claude Code, Cursor, Windsurf, or Trae**) to teach your AI assistant how to write premium, highly-performant glassmorphism layouts and custom 3D scrolling controls.

## 🚀 Features Codified
- ☀️🌙 **Dynamic Light/Dark Theme Balancing:** Highly transparent glass surfaces (`24% White` in Light, `10% White` in Dark) and adaptive outline contrast.
- 🎨 **Seamless Ambient Glow Backdrops:** Multi-orb infinite drifting gradient backgrounds using optimized zero-recomposition `Canvas` draws.
- 🎛️ **Tactile 3D Scrolling Pickers:** Vertical drum wheels and horizontal rulers featuring custom snapping and cylinder `graphicsLayer` projection.
- 📈 **Zero-Recomposition Bezier Charts:** Pre-cached drawing layouts (`drawWithCache`) preventing GC churn during active scroll animations.

## 📦 How to Install and Use

### For Claude Code / Gemini CLI Users:
1. Clone this repository into your project's local agent skills folder:
   ```bash
   git clone https://github.com/rifanalam/compose-glassmorphism-skill.git .agents/skills/compose-glassmorphism-skill
   ```
2. Your AI agent will automatically index `SKILL.md` on startup. Whenever you ask your agent to:
   - *"Create a premium settings card"*
   - *"Build a blood pressure input scroll wheel"*
   - *"Optimize this chart for 120 FPS scrolling"*
   It will automatically activate the skill and follow these guidelines perfectly!

### For Cursor / Windsurf / Trae Users:
Simply copy the contents of `SKILL.md` into your `.cursorrules`, `.windsurfrules`, or project instruction rules, or reference this directory in your custom agent settings!

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
