# three.js-cheat-sheet


## 3d Files
- https://www.cgtrader.com/search?free=1&keywords=
- https://www.turbosquid.com/Search/3D-Models/free/headphone














<br><br>
___
___

<br><br>


# Viewer

## gltf
- https://gltf-viewer.donmccurdy.com/















<br><br>
___
___

<br><br>


# Converter


## GLTF

<br><br>

### obj to glft
- https://convert3d.org/obj-to-gltf

<br><br>

### fbx to gltf
- https://convert3d.org/fbx-to-gltf













<br><br>
___
___

<br><br>

# Init

<br><br>

## Next.js

lib/three.js
```
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'

if (typeof window !== 'undefined') {
    window.THREE = THREE
    window.GLTFLoader = GLTFLoader
}

export default THREE
```



app/provider.tsx:
```typescript
'use client'

import * as React from 'react'

import { NextUIProvider } from '@nextui-org/system'
import { useRouter } from 'next/navigation'
import { ThemeProvider as NextThemesProvider } from 'next-themes'
import { ThemeProviderProps } from 'next-themes/dist/types'

import { useEffect } from 'react'

export interface ProvidersProps {
	children: React.ReactNode;
	themeProps?: ThemeProviderProps;
}

export function Providers({ children, themeProps }: ProvidersProps) {
    const router = useRouter()

    useEffect(() => {
        const loadThree = async() => {
            await import('@/lib/three')
            await import('@/lib/vanta/vanta.globe.js')

            window.VANTA.GLOBE({
                el: '#rootLayout',

                mouseControls: false,
                touchControls: true,
                gyroControls: true,

                // minHeight: 10.00,
                // minWidth: 10.00,

                scale: 1.00,
                scaleMobile: 1.00,

                color: 0xb300ff,

                // globe lines
                // color2: 0xb300ff,

                // 13 will be cross inside square
                spacing: 15,
                points: 8,
                maxDistance: 10,

                // ==== GLOBE ====
                globeSpeed: 0.001,
                globeSize: 1.25,
                globeAmountHeight: 18,
                globeAmountWidht: 20,
                globeHeight: 18,
                globeX: 0,
                globeY: -2.5,
                globeZ: 0,
                globeRotation: -.25,

                // ==== MESH ====
                meshBounceX: 200,

                meshBounceZ: 50,
                meshBounceZMultiplikator: 50,

                meshBounceHeight: 4,
                meshSpeed: 0.002,
                meshOpacity: .1,
                // meshSpacing: 20,
                meshX: -20,
                meshY: 45,
                meshZ: 50,

                // ==== MESH Dots ====
                meshDotSize: .15,
                meshDotOpacity: .6,

                backgroundColor: 0x0
            })
        }
        
        loadThree()
    }, [])

    return (
        <NextUIProvider navigate={router.push}>
            <NextThemesProvider {...themeProps}>{children}</NextThemesProvider>
        </NextUIProvider>
    )
}
```



























<br><br>
<br><br>
___________________________________
___________________________________
<br><br>
<br><br>

# Import

<br><br>

## gltf

```javascript
// ==== ETH LOGO ==== 
loader.load('3d/eth/scene.gltf', gltf => {
    this.ethereumModel = gltf.scene
    this.ethereumModel.scale.set(10, 10, 10) // Skaliert das Modell auf eine angemessene Größe
    this.ethereumModel.position.set(0, 0, 0) // Positioniert das Modell in der Mitte

   // Iteriere über alle Meshes im Modell und ändere deren Materialeigenschaften
    this.ethereumModel.traverse(node => {
	if (node.isMesh) {
	    // Setze das Material auf transparent und ändere die Opazität
	    node.material.transparent = true
	    node.material.opacity = 0.2 // Setze die gewünschte Opazität (0.0 bis 1.0)
	}
    })


    this.cont2.add(this.ethereumModel) // Fügt das Modell der Szene hinzu
})
```

























<br><br>
___
___

<br><br>










# API

<br><br>

## Material 

<br><br>

### MeshBasicMaterial

<br><br>

#### Create wireframe material with neon effect
```javascript
const wireframeMaterial = new THREE.MeshBasicMaterial({
    color: 0x42005e,      // Your desired color in hex
    wireframe: true,      // Enable wireframe mode
    transparent: true,    // Enable transparency
    opacity: 0.8,        // Set transparency level
    emissive: 0x42005e,  // Same color for the glow effect
    emissiveIntensity: 1.5 // Intensity of the glow
});

// Apply to your 3D model
model.traverse(node => {
    if (node.isMesh) {
        node.castShadow = true;
        node.receiveShadow = true;
        node.material = wireframeMaterial;
    }
});
```
- Uses MeshBasicMaterial for the neon effect
- wireframe: true creates the wireframe look
- emissive and emissiveIntensity create the glow effect
- transparent and opacity add depth to the neon look
- Apply to all meshes using traverse








<br><br>
___
___

<br><br>








# Draco Kompression für 3D Modelle

<details><summary>Click to expand..</summary>

## Was ist Draco?
Draco ist eine Open-Source-Bibliothek von Google für die Kompression und Dekompression von 3D-Geometrie-Meshes und Point Clouds. Sie reduziert erheblich die Größe von 3D-Modellen.

## 1. GLTF-Modell komprimieren

```bash
# Installation des gltf-pipeline Tools
npm install -g gltf-pipeline

# Komprimierung eines GLTF-Modells mit Draco
gltf-pipeline -i input.gltf -o model-draco.gltf --draco.compressionLevel=10
```
- **Dadurch ist die bin Datei überflüßig, weil es gibt nur noch die gltf Datei**

Kompressionslevels:
- 1-10 (höher = bessere Kompression, aber längere Verarbeitungszeit)
- 7 ist der Standardwert
- 10 ist maximale Kompression

## 2. Integration in Three.js/React Projekt

### Erforderliche Importe
```javascript
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader.js'
```

### Loader Setup
```javascript
const loader = new GLTFLoader()
const dracoLoader = new DRACOLoader()

// Option 1: Verwendung der Google-hosted Decoder (empfohlen)
dracoLoader.setDecoderPath('https://www.gstatic.com/draco/versioned/decoders/1.5.6/')

// Option 2: Lokale Decoder (erfordert zusätzliche Dateien)
// dracoLoader.setDecoderPath('/draco/')

loader.setDRACOLoader(dracoLoader)
```

### Modell laden
```javascript
async function loadModel() {
    const gltf = await loader.loadAsync('path/to/model-draco.gltf')
    const model = gltf.scene
    // Weitere Verarbeitung des Modells...
}
```

## 3. Vorteile der Draco-Kompression

- Deutlich reduzierte Dateigröße (oft 90% kleiner)
- Schnellere Ladezeiten
- Geringerer Bandbreitenverbrauch
- Bessere Performance für Web-Anwendungen

## 4. Tipps & Tricks

- Verwende die Google-hosted Decoder für einfache Integration
- Teste verschiedene Kompressionslevel für optimale Balance zwischen Größe und Qualität
- Komprimiere nur die finalen Modelle, nicht die Arbeitsversionen
- Behalte die Original-Dateien für spätere Bearbeitungen

## 5. Fehlerbehebung

Häufige Fehler und Lösungen:
- `No DRACOLoader instance provided`: Stelle sicher, dass der DRACOLoader korrekt initialisiert und dem GLTFLoader zugewiesen wurde
- `404 for draco_decoder.js`: Bei lokaler Verwendung müssen alle Decoder-Dateien im richtigen Verzeichnis liegen
- Performance-Probleme: Verwende die Google-hosted Decoder für besseres Caching

## 6. Beispiel-Konfiguration für Three.js

```javascript
// Vollständiges Beispiel
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader.js'

const loader = new GLTFLoader()
const dracoLoader = new DRACOLoader()
dracoLoader.setDecoderPath('https://www.gstatic.com/draco/versioned/decoders/1.5.6/')
loader.setDRACOLoader(dracoLoader)

// Modell laden
const model = await loader.loadAsync('model-draco.gltf')
scene.add(model.scene)
```






</details>



















<br><br>
<br><br>
___________________________________
___________________________________
<br><br>
<br><br>

# Templates

## Objects
- Different objects: https://codepen.io/wakana-k/pen/poBBRmN
