# Mode: product-360 — Product 360° Turntable & Multi-Angle Showcase

Generate product 360° turntable, multi-angle reveal, and product showcase prompts with material fidelity, lighting control, and e-commerce optimization.

## Process

1. Collect context: product category (electronics, jewelry, fashion, beauty, luxury goods, food), key material properties (metal, glass, leather, fabric, ceramic), duration (8–30s), platform format
2. Read `~/.claude/skills/seedance-product-360/references/details.md` for full rotation techniques, material vocabulary, reveal sequence templates, and platform output specs
3. Select showcase type: full 360° turntable · multi-angle reveal sequence · hero beauty shot · exploded view · material macro close-up · dramatic entry reveal
4. Apply 2-second hook: product materializing from void, dramatic spotlight reveal, texture macro that pulls back, particle assembly, material transformation
5. Specify material properties using Seedance vocabulary: reflectivity, specular highlights, roughness, subsurface scattering (for organic/beauty products), fabric drape
6. Describe lighting precisely: key light angle, fill ratio, rim light, background separation, specular highlight position for materials
7. Define rotation: direction (clockwise/counterclockwise), degrees per second, camera height angle, distance from product
8. Specify background: pure white/black/gradient studio, environmental context, seamless floor reflection
9. Include looping behavior if needed (for e-commerce carousels): seamless loop points
10. Build complete prompt: hook + showcase type + material + lighting + rotation + background + loop spec
11. Verify: material properties are specific, lighting specifies reflective surface behavior, rotation direction declared

## Notes
- Subsurface scattering vocabulary is essential for beauty/organic products
- Extended guide: `~/.claude/skills/seedance-product-360/references/details.md`
