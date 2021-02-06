<template>
  <canvas id="pixiCanvas"></canvas>
</template>

<script>
import * as PIXI from 'pixi.js'
import { Viewport } from 'pixi-viewport'
import img from './img.png'

const RATIO = window.devicePixelRatio;

PIXI.settings.ROUND_PIXELS = true;
PIXI.settings.RESOLUTION = RATIO;

export default {  
  name: 'Canvas',
  data () {
    return {
      objects: {},
      viewport: null,
      app: null,
      data: null,
      alpha: 1,
      dragging: false
    }
  },
  mounted() {
    this.setupPixi();
    this.generateTestObjects();
    this.generateObjects();
  },
  methods: {
    setupPixi() {
        const canvas = document.getElementById('pixiCanvas')
        const app = new PIXI.Application({
            view: canvas
        })
        // document.body.appendChild(app.view)

        this.app = app;

        // create viewport
        const viewport = new Viewport({
            screenWidth: window.innerWidth,
            screenHeight: window.innerHeight,
            worldWidth: 1000,
            worldHeight: 1000,

            interaction: app.renderer.plugins.interaction // the interaction module is important for wheel to work properly when renderer.view is placed or scaled
        })

        this.viewport = viewport

        // add the viewport to the stage
        this.app.stage.addChild(this.viewport)

        // activate plugins
        this.viewport
            .drag()
            .pinch()
            .wheel()
            

        // // add a red box
        // const sprite = viewport.addChild(new PIXI.Sprite(PIXI.Texture.WHITE))
        // sprite.tint = 0xff0000
        // sprite.width = sprite.height = 100
        // sprite.position.set(100, 100)
    },
    generateTestObjects() {
        // add a red box
        const texture = new PIXI.Texture.from(img);
        const sprite = this.viewport.addChild(new PIXI.Sprite(texture))
        // sprite.tint = 0xff0000
        sprite.width = sprite.height = 100
        sprite.position.set(100, 100)
    },
    createImageObject(x, y) { 
        // add a red box
        const texture = new PIXI.Texture.from(img);
        const sprite = this.viewport.addChild(new PIXI.Sprite(texture))
        // sprite.tint = 0xff0000
        sprite.width = sprite.height = 100
        sprite.position.set(x, y)
    },
    createTextObject(x, y) {
        // simple text example
        const style = new PIXI.TextStyle({
            fontVariant: "small-caps",
            fontWeight: "bolder",
            fill: "#FFF"
        });
        const text = new PIXI.Text('Hello World', style);
        text.x = x;
        text.y = y;

        // enable the bunny to be interactive... this will allow it to respond to mouse and touch events
        text.interactive = true;

        // this button mode will mean the hand cursor appears when you roll over the bunny with your mouse
        text.buttonMode = true;

        // center the bunny's anchor point
        text.anchor.set(0.5);

        // make it a bit bigger, so it's easier to grab
        text.scale.set(0.3);

        // setup events
        text
            // events for drag start
            .on('mousedown', this.onDragStart)
            .on('touchstart', this.onDragStart)
            // events for drag end
            .on('mouseup', this.onDragEnd)
            .on('mouseupoutside', this.onDragEnd)
            .on('touchend', this.onDragEnd)
            .on('touchendoutside', this.onDragEnd)
            // events for drag move
            .on('mousemove', this.onDragMove)
            .on('touchmove', this.onDragMove);

        // move the sprite to its designated position
        text.position.x = x;
        text.position.y = y;

        this.viewport.addChild(text);
    },
    generateObjects() {
        for (var i = 0; i < 1000; i++)
        {
            this.createTextObject(this.adjustedXForRatio() , this.adjustedYForRatio());
            this.createImageObject(this.adjustedXForRatio() , this.adjustedYForRatio());
        }
    },
    adjustedXForRatio() {
        const value = (Math.floor(Math.random() * 10920)) / RATIO
        // console.log(value)
        return value
    },
    adjustedYForRatio() {
        const value = (Math.floor(Math.random() * 6080)) / RATIO
        // console.log(value)
        return value
    },
    onDragStart(event) {
        // store a reference to the data
        // the reason for this is because of multitouch
        // we want to track the movement of this particular touch
        this.viewport.pressDrag = false;
        this.data = event.data;
        // console.log(this.data);
        this.alpha = 0.5;
        this.dragging = true;
    },
    onDragEnd() {
        this.alpha = 1;

        this.dragging = false;
        this.viewport.pressDrag = true;

        // set the interaction data to null
        this.data = null;
    },
    onDragMove() {
        if (this.dragging)
        {
            // console.log(this.data);

            // var newPosition = this.data.getLocalPosition(this.parent);
            // this.position.x = newPosition.x;
            // this.position.y = newPosition.y;
        }
    }
  }
}
</script>

<style>

    #pixiCanvas {
        display: block;
        background-color: blue;
        height: 100vh - 64px;
        width: 100%;
        margin-top: 64px;
        overflow: hidden;
    }
</style>