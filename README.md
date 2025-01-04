# three.js-cheat-sheet


## 3d Files
- [gltf](https://www.cgtrader.com/search?free=1&keywords=headphone)






<br><br>
___
___

<br><br>


# Create gltf files

<br><br>

## obj to glft
- https://convert3d.org/obj-to-gltf

<br><br>

## fbx to gltf
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
<br><br>
___________________________________
___________________________________
<br><br>
<br><br>

# Templates

## Objects
- Different objects: https://codepen.io/wakana-k/pen/poBBRmN
