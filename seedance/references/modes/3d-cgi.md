# Mode: 3d-cgi — 3D CGI & Rendered Video

Generate 3D CGI and rendered video prompts: Blender, Unreal Engine 5, Octane, particle systems, creature animation, product visualization.

## Process

1. Collect context: render style target (Pixar, UE5 photorealistic, architectural, sci-fi), subject, duration, platform ratio
2. Read `~/.claude/skills/seedance-3d-cgi/references/details.md` for full render engine terminology and technique library
3. Select render engine vocabulary (Cycles, Octane, V-Ray, Arnold, etc.) matching user's target aesthetic
4. Apply 2-second hook: choose from freeze frame, particle burst, scale reveal, or material close-up hooks
5. Specify material properties: roughness/metallic, SSS, translucency, anisotropy as relevant
6. Describe lighting: HDRI, GI, caustics, volumetric lighting, shadow behavior
7. Add camera movement: DOF, motion blur, tracking shot, crane, or orbital
8. Build complete prompt with render terminology, material, lighting, camera in sequence
9. Verify: prompt is specific enough (no vague descriptors), platform ratio declared, duration appropriate

## Notes
- Key constraint: vague prompts fail on Seedance CGI — must use render engine terminology
- Extended guide: `~/.claude/skills/seedance-3d-cgi/references/details.md`
