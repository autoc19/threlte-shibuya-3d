<script lang="ts">
  import { T } from '@threlte/core'
  import { ContactShadows, Float, Grid, OrbitControls } from '@threlte/extras'
  import type * as THREE from 'three'
  import BoxExModel from './models/box_ex.svelte'
  import BoxExNewModel from './models/box_ex_new.svelte'
  import ShibuyaModel from './models/shibuya.svelte'

  let focusedModel: string | null = $state(null)
  let orbitControlsInstance: any

  const focusOnModel = (modelName: string, position: [number, number, number]) => {
    console.log('Focusing on model:', modelName, 'at position:', position)
    focusedModel = modelName
    if (orbitControlsInstance) {
      orbitControlsInstance.target.set(...position)
      orbitControlsInstance.update()
    } else {
      console.log('OrbitControls instance not available:', orbitControlsInstance)
    }
  }

  const handleShibuyaClick = () => {
    console.log('Shibuya model clicked')
    focusOnModel('shibuya', [0, 0.705, 0])
  }
  const handleBoxExClick = () => {
    console.log('BoxEx model clicked')
    focusOnModel('boxex', [4, 1, -3])
  }
  const handleBoxExNewClick = () => {
    console.log('BoxExNew model clicked')
    focusOnModel('boxexnew', [-4, 1, 3])
  }
</script>

<T.PerspectiveCamera
  makeDefault
  position={[-15, 15, 15]}
  fov={45}
>
  <OrbitControls
    oncreate={(controls) => {
      orbitControlsInstance = controls
      console.log('OrbitControls created:', controls)
    }}
    enableZoom={true}
    enableDamping
    autoRotateSpeed={0.5}
    target.y={1.5}
    minDistance={5}
    maxDistance={50}
    minPolarAngle={0}
    maxPolarAngle={Math.PI / 2}
  />
</T.PerspectiveCamera>

<T.DirectionalLight
  intensity={0.8}
  position.x={5}
  position.y={10}
/>
<T.AmbientLight intensity={0.2} />

<Grid
  position.y={-0.001}
  cellColor="#ffffff"
  sectionColor="#ffffff"
  sectionThickness={0}
  fadeDistance={25}
  cellSize={2}
/>

<ContactShadows
  scale={10}
  blur={2}
  far={2.5}
  opacity={0.5}
/>

<ShibuyaModel 
  position={[0, 0.705, 0]} 
  onClick={handleShibuyaClick}
  isFocused={focusedModel === 'shibuya'}
/>
<BoxExModel 
  position={[4, 1, -3]} 
  onClick={handleBoxExClick}
  isFocused={focusedModel === 'boxex'}
/>
<BoxExNewModel 
  position={[-4, 1, 3]} 
  onClick={handleBoxExNewClick}
  isFocused={focusedModel === 'boxexnew'}
/>


