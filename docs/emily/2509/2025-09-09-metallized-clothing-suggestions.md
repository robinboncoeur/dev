# Metallized clothing suggestions

Date: 2025-09-09

---

## Robyn

Hi Emily... I'm trying to come up with a word to put in my Flux1 Dev prompt to indicate all clothing has to have a surrealistic almost metalic shine to them. How would I do this?

---

## Emily

G’day, Robyn! A single word that works well is **“metallized.”**  
Other strong, compact tokens: **chrome**, **iridescent**, **pearlescent**, **lamé**, **anodized**, **mirror-finish**, **patent**, **wet-look**, **reflective**, **specular**.

Quick plug-ins you can drop in (pick one and repeat once near the end):

- **“metallized clothing”**  
- **“chrome-sheen garments”**  
- **“iridescent, mirror-finish fabric”**  
- **“anodized metallic sheen”**  
- **“holographic lamé texture”**

A tidy Flux.1 Dev phrasing that tends to stick globally:

> *“all clothing is **metallized** with a **chrome-like, iridescent** sheen, **high specular highlights**, **reflective**, **no matte fabrics**, **no cotton/wool/knit**”*

If it’s still too matte, weight the key bits:  
`((metallized)), ((chrome)), ((iridescent)), reflective, high specular highlights, no matte`

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Suggesting keywords for metallic sheen",
      "content": "For the user’s image gen prompt, good keywords for describing \"surrealistic metallic shine\" could include: \"iridescent,\" \"chrome-like,\" \"anodized,\" \"specular,\" \"latex-like gloss,\" \"pearlescent,\" and \"holographic.\" \"Liquid-metal\" or \"chromed\" might also work well, as they often capture that unreal, metallic gloss. These should get the effect they’re looking for!",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Guidance on generating metallic sheen clothing",
      "content": "For a surreal metallic look in Flux.1 Dev, you can try prompts like \"surreal chrome-sheen clothing,\" \"iridescent pearlescent anodized finish,\" or \"vacuum-metallized garments.\" Focus on global style directives for high specular finishes, avoiding matte or fabric textures. Use weighting for terms like \"chrome,\" \"iridescent,\" or \"reflective,\" and try terms like \"lamé\" for metallic fabric.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Providing clothing-related advice",
      "content": "For the clothing prompt, suggest terms like \"iridescent,\" \"chrome,\" and \"metallic sheen.\" Include terms like \"lamé\" or \"PVC\" to suggest futuristic fabrics, but keep the tone neutral to avoid any inappropriate associations. You can also suggest additional phrases like \"specular highlights\" for detail. Provide example prompts like, \"All garments have an iridescent metallic sheen with no matte textures,\" and a weighted tag such as ((chrome)) or ((reflective)) for extra emphasis.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Flux.1 Dev clothing prompt advice",
      "content": "For a surreal, metallic look in Flux.1 Dev, suggest terms like \"metallized,\" \"chrome,\" \"iridescent,\" and \"anodized.\" Use phrases such as \"metallized clothing with chrome-like sheen\" or \"surreal, mirror-finish garments with high specular highlights.\" Don’t forget to include negative prompts like \"no matte fabrics\" and consider weighting for \"chrome\" or \"reflective\" to emphasize the metallic sheen.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "388e14f0-d456-42b4-9849-2db4ce3ce71e"
}

---

## Emily

Thought for 38s

---

## Robyn

Thank you - yes, I think the holographic lame texture should do it. Thank you.

---

## Emily

Perfect! One tiny note: it’s spelled **lamé** (with the accent), which sometimes helps parsers.

Here’s a compact drop-in that usually sticks globally:

**Positive:**  
`all clothing in holographic lamé, chrome-like sheen, high specular highlights, reflective, iridescent diffraction, anisotropic shimmer, fabric not armor`

**Negative (to kill matte stuff):**  
`matte, cotton, wool, knit, linen, fleece, velvet, rough, diffuse, low specular`

Tips:
- Repeat **“holographic lamé”** once near the end or weight it: `((holographic lamé))`.
- If it gets too rainbowy, anchor with: `silver base with rainbow diffraction`.
- To keep it textile (not hard metal): add `soft fabric, no hard metal, not armor`.

Shout if you want me to tailor this to your exact ComfyUI graph or LoRA tags.

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Providing helpful follow-up",
      "content": "Got it! Just a quick reminder that it's \"lamé\" with the accent—it's a small detail! You’ve also got good coverage with \"holographic lamé.\" If you want to switch it up, try terms like “holographic foil fabric” or “mylar.” You might also explore alternate nodes like \"specular\" in ComfyUI. Let me know if you need more!",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Providing parameter tips",
      "content": "For that surrealistic metallic shine, try emphasizing specular in your model. If the matte effect isn't desired, use negative terms like cotton, linen, or wool. For unwanted leather shine, consider using \"nonleather\" instead. Try \"global: holographic lamé fabric\", and weight tokens both early and late for better results.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "4f5c9085-0998-4e9e-b05a-08af90fc846a"
}

---

## Emily

Thought for 23s

---
