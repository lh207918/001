# 001
import { Engine, Model } from "reze-engine"

export default function Scene() {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const engineRef = useRef<Engine>(null)

  const initEngine = useCallback(async () => {
    if (canvasRef.current) {
      try {
        const engine = new Engine(canvasRef.current, {})
        engineRef.current = engine
        await engine.init()
        const model = await engine.loadModel("/models/reze/reze.pmx")
        await model.loadAnimation("default", "/animations/dance.vmd")
        model.resetAllBones()
        model.resetAllMorphs()
        model.show("default")
        model.play()

        engine.runRenderLoop(() => {})
      } catch (error) {
        console.error(error)
      }
    }
  }, [])

  useEffect(() => {
    void initEngine()
    return () => engineRef.current?.dispose()
  }, [initEngine])

  return <canvas ref={canvasRef} className="w-full h-full" />
}
{
  ambientColor: new Vec3(0.88, 0.88, 0.88),
  directionalLightIntensity: 0.24,
  minSpecularIntensity: 0.3,
  rimLightIntensity: 0.4,
  cameraDistance: 26.6,
  cameraTarget: new Vec3(0, 12.5, 0),
  cameraFov: Math.PI / 4,
  onRaycast: (modelName, material, screenX, screenY) => { /* tap/click on model */ },
  multisampleCount: 4,  // 1 | 4
}
const model = await engine.loadModel("hero", "/path/to/model.pmx")
engine.getModelNames()
engine.getModel("hero")
engine.removeModel("hero")
// Or add an existing Model: await engine.addModel(model, pmxPath, "hero")

engine.setMaterialVisible("hero", "材質1", false)
engine.setIKEnabled(true)     // IK enabled for all models
engine.setPhysicsEnabled(true) // physics enabled for all models
engine.resetPhysics()         // resets physics for all instances
engine.markVertexBufferDirty("hero")  // or pass Model
const model = engine.getModel("hero")

// Single animation (e.g. dance): load, then play when needed
await model.loadAnimation("default", "/animations/dance.vmd")
model.resetAllBones()
model.resetAllMorphs()
model.show("default")
model.play()
const { current, duration, percentage } = model.getAnimationProgress()

// Multiple named animations
await model.loadAnimation("idle", "/animations/idle.vmd")
await model.loadAnimation("walk", "/animations/walk.vmd")
model.show("idle")         // show "idle" at time 0, no playback
model.play("walk")          // play "walk"; if something is playing, "walk" is queued for when it ends
model.play()                 // resume current
model.pause()
model.stop()
model.seek(1.5)
model.getAnimationProgress()  // { current, duration, percentage, animationName }
model.getAnimationState().setOnEnd((name) => model.play("idle"))
model.rotateBones({ 首: neckQuat, 頭: headQuat }, 300)
model.moveBones({ センター: centerVec }, 300)
model.setMorphWeight("まばたき", 1.0, 300)
model.resetAllBones()
model.resetAllMorphs()
engine.runRenderLoop(() => {})

engine.addGround({ width: 160, height: 160, mode: "reflection", ... })  // mirror + reflection
engine.addGround({ width: 160, height: 160, mode: "shadow", ... })      // floor + character shadow

// Raycast: tap/click on model (callback receives model name and material name)
new Engine(canvas, {
  onRaycast: (modelName, material, screenX, screenY) => { ... }
})
