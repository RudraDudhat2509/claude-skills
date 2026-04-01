License
Complete terms in LICENSE.txt

This skill guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic details, creative choices, and rich motion.

The user provides frontend requirements: a component, page, application, or interface to build. They may include context about the purpose, audience, or technical constraints.

Step 1: Design Thinking
Before writing a single line of code, commit to a bold aesthetic direction. Answer these internally:

Purpose: What problem does this interface solve? Who uses it?
Tone: Pick ONE extreme and own it fully. See the Aesthetic Menu below.
Motion Signature: What is the ONE animation moment that will make this feel alive? Decide this upfront.
Differentiation: What makes this UNFORGETTABLE? If you can't answer this, redesign.
CRITICAL: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work — the key is intentionality, not intensity. Wishy-washy middle-ground designs are the enemy.

Step 2: Aesthetic Menu (pick one, commit fully)
Use these as springboards — execute one with full conviction, don't blend them into mush:

Aesthetic	Visual Signature	Motion Style
Brutalist/Raw	Heavy borders, stark contrast, exposed grid, monospace	Snap transitions, no easing, instant state changes
Luxury/Editorial	Wide tracking, thin serifs, gold/cream, generous whitespace	Slow fades, silk curtain reveals, elegant stagger
Retro-Futuristic	Scanlines, CRT glow, phosphor greens/ambers, grid overlays	Flicker-in, typewriter, glitch bursts
Organic/Natural	Earth tones, irregular shapes, hand-drawn elements, noise textures	Breathing animations, gentle float, physics-like spring
Glassmorphism/Neo	Frosted glass, deep blur, vibrant glow, dark base	Shimmer sweeps, glow pulses, liquid morphs
Toylike/Playful	Bold primaries, chunky rounded corners, cartoon shadows	Bounce, squish/stretch, pop, wiggle
Art Deco/Geometric	Symmetry, gold lines, chevrons, jewel tones	Mechanical reveals, iris wipe, rotational entrance
Vaporwave/Y2K	Pastels, chrome text, gradients, pixelated accents	Scanline scroll, chromatic aberration flicker
Cinematic/Dark	Deep blacks, film grain, muted palettes, dramatic lighting	Scene-cut transitions, spotlight reveals, parallax
Industrial/Utilitarian	Monochrome, system fonts used deliberately, data-dense	Mechanical slide, gauge fills, data stream
FORBIDDEN convergence: Space Grotesk + purple gradient + card grid + subtle hover shadow. This combination is banned. So is: Inter + white background + blue CTA + minimal animations.

Step 3: Typography Rules
NEVER use: Inter, Roboto, Arial, system-ui, -apple-system, Helvetica, Open Sans
Always import from Google Fonts or use a web-safe serif with intention
Pair a distinctive display font (headlines) with a refined text font (body)
Use font-feature-settings, letter-spacing, text-transform aggressively for character
Variable fonts with weight animations on hover are encouraged
Good font combos by aesthetic:

Brutalist: Bebas Neue + Courier New (or IBM Plex Mono)
Luxury: Cormorant Garamond + Jost
Retro-Futurism: Orbitron + Share Tech Mono
Organic: Playfair Display + Lora
Playful: Fredoka One + Nunito
Editorial: DM Serif Display + DM Sans
Step 4: Animation System (MANDATORY — not optional)
Every output MUST have a motion system. "Simple" requests still get motion — just restrained motion.

Page Load Sequence (always implement this)
Every interface must have an entrance. Stagger elements with animation-delay:

css
/* HTML/CSS pattern */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

.hero-title    { animation: fadeUp 0.6s cubic-bezier(0.16,1,0.3,1) 0.1s both; }
.hero-subtitle { animation: fadeUp 0.6s cubic-bezier(0.16,1,0.3,1) 0.25s both; }
.hero-cta      { animation: fadeUp 0.6s cubic-bezier(0.16,1,0.3,1) 0.4s both; }
Scroll Animations (use IntersectionObserver for HTML)
javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) e.target.classList.add('visible');
  });
}, { threshold: 0.15 });

document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
css
.reveal { opacity: 0; transform: translateY(32px); transition: all 0.7s cubic-bezier(0.16,1,0.3,1); }
.reveal.visible { opacity: 1; transform: none; }
Hover States (always have at least 2 of these)
Background color sweep (clip-path or pseudo-element)
Underline draw (scaleX from 0 to 1)
Icon/arrow nudge (translateX 4-6px)
Border glow pulse
Magnetic cursor pull (JS)
Text color split (clip-path)
Scale + shadow lift
Aesthetic-Specific Animations by style:
Retro/CRT: @keyframes flicker with opacity jitter; @keyframes scanline moving gradient
Glassmorphism: Shimmer sweep (background-position animation); glow pulse on box-shadow
Brutalist: No easing — use steps() for mechanical feel; instant color swaps on hover
Luxury: Slow cubic-bezier(0.76,0,0.24,1) on everything; silk curtain clip-path reveals
Playful: @keyframes bounce with cubic-bezier(0.34,1.56,0.64,1) (overshoot); squish on click
Organic: @keyframes float with gentle translateY loop; SVG path morphing
React Motion (when using React)
Use framer-motion / motion library:

jsx
import { motion } from "framer-motion";

const container = {
  hidden: { opacity: 0 },
  show: { opacity: 1, transition: { staggerChildren: 0.1 } }
};
const item = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0, transition: { type: "spring", bounce: 0.4 } }
};

<motion.ul variants={container} initial="hidden" animate="show">
  {items.map(i => <motion.li key={i} variants={item}>{i}</motion.li>)}
</motion.ul>
Step 5: Color & Visual Depth
Use CSS custom properties for every color value — no hardcoded hex in component rules
One dominant color, one accent, one neutral — three-color palettes beat rainbow chaos
Dominant colors with sharp accents outperform timid, evenly-distributed palettes
Background is never plain white or plain black — always add:
Noise texture (filter: url(#noise) or CSS url("data:image/svg+xml..."))
Gradient mesh (radial-gradient layered 2-3 times at different positions)
Subtle grid or dot pattern
Film grain overlay (opacity: 0.03-0.08 on a noise pseudo-element)
Step 6: Spatial Composition
Break the grid on purpose:

Overlapping elements with z-index and negative margins
Diagonal dividers (clip-path: polygon(...))
Sticky/parallax layers that move at different speeds
Full-bleed sections that break out of content containers
Asymmetric layouts: 60/40 splits, off-center hero text, edge-anchored elements
Decorative elements that bleed off-screen
Step 7: Component Patterns (use these, not generic cards)
Instead of card with shadow + title + description + button, use:

Generic	Distinctive alternative
Card grid	Masonry with staggered heights + hover tilt
Nav bar	Morphing pill nav that tracks active item
Hero section	Full-bleed with layered parallax + text mask reveal
Button	Magnetic button with cursor pull effect
Form input	Animated label float + colored focus border sweep
Loading state	Custom skeleton with shimmer OR thematic spinner
Modal	Backdrop blur + scale-in from click origin
Tooltip	Follows cursor, physics-based position
Progress bar	Liquid fill with wave animation
Toggle	Morphing icon + spring snap
Anti-Generic Checklist (run this before outputting)
Before finalizing, verify:

 No Inter, Roboto, Arial, or system fonts
 No purple gradient on white/light background
 No generic card-grid layout as the primary pattern
 At least one page-load animation sequence with stagger
 At least two hover interaction states
 Background has texture, gradient, or depth — not flat color
 The aesthetic can be named in one word (brutalist, luxe, retro, etc.)
 Someone could not generate this same output by prompting "make a landing page"
Final Principle
Claude is capable of extraordinary creative work. Don't hold back. Every output should feel like it was designed by someone with a strong, specific point of view — not assembled from a component library. The goal is not "looks good" — it's "I've never seen this before and I can't stop looking at it."
