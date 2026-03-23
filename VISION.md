# Night Shift — v2 Visual Vision

Filed: 2026-03-22. Build this when the core loop is proven.

## Dynamic Lighting Engine (the multiplier)

Implement 2D dynamic lighting via canvas compositing:
- Render world fully to canvas
- Draw near-opaque black overlay on second offscreen canvas
- Use `destination-out` blend mode to punch soft radial/conical holes for each light source
- Composite darkness layer on top of world layer

**Light sources:**
- Streetlamps: warm yellow radial, ~80px radius, one per intersection
- Neon signs: colored rectangular glow patches on ground beneath sign
- Car headlights: forward-facing conical, rotates with vehicle angle
- Taillights: small red radial rear-facing
- Police lights: alternating red/blue flash that casts colored light on surroundings
- Cigarette cherry: tiny orange point light on pedestrian idle
- Muzzle flash: momentary bright white burst (point light)
- Open doorways: rectangle of warm light spilling onto sidewalk

Light colors tint ground and sprites beneath them. Unlit areas = silhouettes only, not "slightly dim daytime."

## Rain System

Persistent light rain:
- Small animated splash sprites on road/sidewalk tiles
- Puddle reflections: flip and reduce-opacity nearby neon colors below their source
- This is the single biggest atmosphere multiplier per spec

## Particle Systems

- Cigarette smoke: small wispy gray sprites on pedestrian idle
- Steam from grates: larger, slower, translucent white drifting up
- Wet tire tracks: fade over ~3 seconds
- Neon flicker: randomized brightness oscillation on ~20% of signs

## Character Sprites (v2 redesign)

12-16px top-down sprites with:
- Distinct silhouettes per type: walker, drunk, dealer, pedestrian
- Idle animations: leaning on walls, stumbling, smoking
- Lighting response: brighten under streetlamps, darken toward silhouette in shadows
- Player sprite: slightly larger, subtle rim-light edge for readability in dark

## Vehicle Sprites

- Always-on headlight cones (two small white conical lights)
- Taillight glow (red)
- Taxi with roof light
- Cop car with red/blue alternating flash

## Environment Additions

- Steam/fog from street grates
- Alley: single bare-bulb light source, very dark otherwise
- Window toggle: some lit (warm yellow), some TV-blue, randomized
- Storefronts as light sources: BAR, MOTEL, LIQUOR signs cast ground glow

## HUD Redesign

- Pager/phone screen aesthetic
- Slight amber CRT tint
- Health/status as small icons, not bars

## Camera (DEFERRED — not worth the architecture cost)

The 3/4 oblique camera with building-face perspective is a full coordinate system change. 
Touches: collision, camera math, tile rendering, sprite layering.
Current flat top-down is correct for this fidelity level. Drop this permanently.

## References

- GTA1: structure, tile scale, sprite scale
- Teleglitch: darkness/visibility mechanic  
- Hong Kong neon alley photography: color palette
- Blade Runner: mood
- Hotline Miami: top-down energy (darker, slower-paced version)

## Technical Notes

- Native render: 800x600, pixel-perfect, no sub-pixel smoothing on sprites
- Canvas 2D is sufficient — no WebGL required for this lighting approach
- Offscreen buffer + compositing adds ~2-3ms per frame at this resolution
- Rain reflections are the cheapest high-impact addition after the darkness layer

---

Build order when ready:
1. Darkness overlay + light punch-through (core engine)
2. Neon sign light casting
3. Streetlamp pools
4. Rain + puddle reflections
5. Car headlight/taillight cones
6. Particles (steam, smoke)
7. Sprite lighting response
8. Character sprite redesign
