<template>
	<h1>{{ msg }}</h1>

	<canvas id="reder-canvas" ref="renderCanvas" />
</template>

<script lang="ts">
import { ref, defineComponent, onMounted } from "vue";

import { Engine } from "@babylonjs/core/Engines/engine";
import { Scene } from "@babylonjs/core/scene";
import { MeshBuilder } from "@babylonjs/core/Meshes/meshBuilder";
import { ArcRotateCamera } from "@babylonjs/core/Cameras/arcRotateCamera";
import "@babylonjs/core/Helpers/sceneHelpers";

export default defineComponent({
	name: "HelloWorld",
	props: {
		msg: {
			type: String,
			required: true,
		},
	},
	setup: () => {
		const renderCanvas = ref();

		const createScene = function () {
			const engine = new Engine(renderCanvas.value, true);
			const scene = new Scene(engine);

			MeshBuilder.CreateBox("box", { size: 1 }, scene);
			scene.createDefaultCameraOrLight(true, true, true);
			(scene.activeCamera as ArcRotateCamera).beta -= 0.4;
			scene.createDefaultEnvironment();

			engine.runRenderLoop(() => scene.render());

			debug(scene);
		};

		const debug = async (scene: Scene) => {
			await import("@babylonjs/inspector");
			await import("@babylonjs/core/Debug/debugLayer");
			scene.debugLayer.show();
		};

		onMounted(() => {
			createScene();
		});

		return { renderCanvas };
	},
});
</script>

<style scoped>
#render-canvas {
	width: 50%;
	height: 50%;
}
</style>
