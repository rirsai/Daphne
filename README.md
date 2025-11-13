# Metallic Fluid WebGL Synth 🎨🎵

An interactive audio-visual synthesizer that combines WebGL fluid simulation with music theory-based sound generation. Paint with colors that each have their own unique sound and musical personality.

## Overview

This project is a **real-time audio-visual instrument** where:
- **Visual**: WebGL-based fluid dynamics simulation responds to cursor movement
- **Audio**: Web Audio API generates musical sounds quantized to proper scales
- **Interaction**: Each color represents a different musical scale and sonic texture

## Features

### 🎨 Color Voices (Musical Scales)

Each color uses a different musical scale, giving it a unique melodic character:

| Color | Scale | Character | Sound Texture |
|-------|-------|-----------|---------------|
| **Grey** | Pentatonic | Safe, balanced | Clean sine wave |
| **Brown** | Blues | Earthy, soulful | Crackles, distortion, sub-bass |
| **Blue** | Minor | Sad, emotional | Modulated sine waves |
| **Yellow** | Major | Bright, happy | Bright sawtooth, high frequencies |
| **Green** | Pentatonic | Natural, organic | Bamboo wind chimes |
| **Pink** | Harmonic Minor | Exotic, crystalline | Ice crystals, glass shatter |
| **Red** | Chromatic | Chaotic, industrial | Metal grinding, heavy distortion |

### 🎼 Musical Quantization System

All frequencies are **quantized to proper musical notes** using:
- **A440 Standard**: Universal tuning where A = 440 Hz
- **Equal Temperament**: 12-tone system, each semitone = 2^(1/12)
- **MIDI Note Mapping**: Cursor Y position maps to MIDI notes (48-84 range)
- **Scale Quantization**: Notes snap to the selected color's scale

**How it works:**
1. Cursor Y position → Raw frequency
2. Raw frequency → Nearest MIDI note
3. MIDI note → Quantized to scale
4. Result: Always musical, never out of tune!

### 🥁 Rhythmic Beat System

Four beat patterns with proper musical timing:

| Beat | Icon | Division | Description |
|------|------|----------|-------------|
| **Kick** | ● | Quarter notes | Every beat (1, 2, 3, 4) - Root note |
| **Hi-hat** | × | Eighth notes | Double time (1-and-2-and) - High frequency noise |
| **Snare** | ▬ | Half notes | Backbeat (beats 2 & 4) - Perfect fifth |
| **Pulse** | ◉ | Whole notes | Once per bar - Root note with reverb |

All beats sync to **120 BPM master tempo** and use **musical intervals** (root, fifth) that harmonize with the colors.

### ✨ Glitch Effects

Five visual and audio manipulation effects:

| Glitch | Icon | Effect |
|--------|------|--------|
| **Bit Crush** | ◼ | Extreme pixelation, bit-crushed audio, color quantization |
| **Corrupt** | ◆ | Color inversion, pitch jumps, velocity spikes |
| **Feedback** | ↻ | Visual trails, audio delay/feedback |
| **RGB Split** | ▣ | Prism effect - colors split into rainbow layers |
| **Freeze** | ❄ | Darkens fluid, freezes sound, icy blue tint |

All glitches are **cursor-responsive** and can be layered for complex effects.

## Technical Architecture

### WebGL Fluid Simulation

**Shaders:**
- `advectionProgram`: Moves fluid based on velocity field
- `diffusionProgram`: Spreads colors through the fluid
- `splatProgram`: Injects color and velocity at cursor position
- `displayProgram`: Renders the final fluid to screen

**Framebuffers:**
- `velocity`: Stores fluid velocity vectors (double-buffered)
- `dye`: Stores color information (double-buffered)
- Uses floating-point textures for smooth simulation

**Parameters:**
- `simWidth/simHeight`: 128×128 simulation resolution
- `persistence`: How long colors stay visible (per-color)
- `diffusion`: How much colors spread (per-color, modified by glitches)

### Web Audio API Architecture

**Signal Flow:**
```
Oscillator → Envelope → Filter → [Glitches] → Panner → Dry/Wet Mix → Master → Output
                                                            ↓
                                                         Reverb
```

**Audio Nodes:**
- **Oscillators**: Generate base frequencies (sine, triangle, sawtooth)
- **Envelopes**: Control volume over time (attack/release)
- **Filters**: Shape frequency content (lowpass, highpass, bandpass)
- **Effects**: Reverb, delay, distortion, bit crushing
- **Panner**: Stereo positioning based on cursor X position

**Special Sound Generators:**
- Bamboo chimes (Green): Multiple harmonically-tuned oscillators
- Ice crystals (Pink): Rapid high-frequency crackles with frozen resonance
- Metal grinding (Red): Noise + modulated oscillators with heavy distortion
- Crackles (Brown): Randomized short bursts for organic texture

### Music Theory Implementation

**Core Functions:**

```javascript
midiToFreq(midiNote)
// Converts MIDI note number to Hz: 440 * 2^((note - 69) / 12)

quantizeToScale(rawFreq, scaleName, rootNote)
// Snaps any frequency to nearest note in the specified scale

quantizePitch(mouseYPos)
// Maps cursor Y → MIDI note → quantized frequency using color's scale
```

**Scales Implemented:**
- Pentatonic: [0, 2, 4, 7, 9] - 5 notes
- Major: [0, 2, 4, 5, 7, 9, 11] - 7 notes (happy)
- Minor: [0, 2, 3, 5, 7, 8, 10] - 7 notes (sad)
- Blues: [0, 3, 5, 6, 7, 10] - 6 notes (soulful)
- Harmonic Minor: [0, 2, 3, 5, 7, 8, 11] - 7 notes (exotic)
- Chromatic: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11] - All 12 notes

## File Structure

```
MetalicFluidWebGLSynth/
├── index.html          # Single-file application (all code)
├── README.md           # This file
└── .git/               # Git repository
```

The entire application is contained in a single `index.html` file (~3300 lines) with:
- HTML structure (lines 1-195)
- CSS styling (lines 7-187)
- JavaScript (lines 196-3325):
  - WebGL setup and shaders (lines 196-400)
  - Color voice definitions (lines 229-412)
  - Musical quantization system (lines 423-515)
  - Beat system (lines 517-599)
  - Glitch effects (lines 405-761)
  - Audio generation (lines 1848-2500+)
  - Rendering loop (lines 2750-2900+)

## How to Use

### Getting Started

1. Open `index.html` in a modern web browser (Chrome, Firefox, Safari)
2. Click **"Sound On"** to enable audio
3. Select a color from the palette
4. Move your cursor around the canvas to paint with sound

### Controls

**Color Selection:**
- Click any color swatch to select it
- Selected color has a white border
- Each color has a unique sound and scale

**Beat System:**
- Click beat buttons to toggle rhythms
- Active beats pulse and have white borders
- Multiple beats can be layered

**Glitch Effects:**
- Click glitch buttons to toggle effects
- Active glitches have white borders
- Effects respond to cursor movement
- Can be combined for complex results

### Performance Tips

- **Cursor Speed**: Faster movement = more energetic sounds and visuals
- **Cursor Position**: 
  - Y-axis controls pitch (top = high, bottom = low)
  - X-axis controls stereo panning (left/right)
- **Layering**: Switch colors while painting to layer different scales
- **Beats**: Start with kick, add hi-hat for groove
- **Glitches**: Try one at a time first, then combine

## Development

### Branch Structure

- `main`: Stable releases with all features
- `glitch-and-beat-system`: Development of glitch/beat features
- `Harmony`: Music theory and quantization system (current)

### Key Technologies

- **WebGL**: Hardware-accelerated fluid simulation
- **Web Audio API**: Real-time audio synthesis
- **Vanilla JavaScript**: No frameworks, pure web standards
- **CSS3**: Modern styling with animations

### Musical Settings

Current settings (configurable in code):

```javascript
musicalSettings = {
    rootNote: 60,        // Middle C (MIDI 60 = 261.63Hz)
    scale: 'pentatonic', // Default scale (overridden by colors)
    octaveRange: 3       // 3 octaves range (MIDI 48-84)
}

musicalClock = {
    bpm: 120,           // Master tempo
    beatsPerBar: 4      // 4/4 time signature
}
```

## Browser Compatibility

**Recommended:**
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**Requirements:**
- WebGL support
- Web Audio API support
- Floating-point texture support

## Credits

Created as an exploration of interactive audio-visual synthesis combining:
- Fluid dynamics simulation
- Music theory and quantization
- Real-time audio synthesis
- Glitch aesthetics

## License

This project is provided as-is for educational and creative purposes.

---

**Version**: Harmony Branch (Musical Quantization System)  
**Last Updated**: November 2025
