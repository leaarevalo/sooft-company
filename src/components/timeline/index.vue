<template>
  <section>
    <h3>
      <b>Timeline</b>
    </h3>
    <div id="visualization" ref="vis">
      <div class="menu">
        <input
          style="display: none"
          id="sliderZoom"
          type="range"
          class="range"
          name="a"
          min="-1"
          max="1"
          step="0.1"
          value="0"
          @input="zoomTimeline"
        />
        <i
          class="material-icons dp48 buttons-menu"
          @click="resetTimeline"
          id="fit"
          >home</i
        >
        <i
          class="material-icons dp48 buttons-menu"
          @click="moveLeft"
          id="moveLeft"
          >arrow_back</i
        >
        <i
          class="material-icons dp48 buttons-menu"
          @click="moveRight"
          id="moveRight"
          >arrow_forward</i
        >
      </div>
    </div>
  </section>
</template>

<script>
import moment from "moment";
import vis from "vis";
/**
 * Move the timeline a given percentage to left or right
 * @param {Number} percentage   For example 0.1 (left) or -0.1 (right)
 */
export default {
  name: "timeline",
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      timeline: null,
      data: [],
      timelineOptions: {
        orientation: {
          axis: "top",
          item: "top",
        },
        zoomMax: 87600900,
        zoomMin: 10000000,
      },
    };
  },
  mounted() {
    this.initTimeline();
  },
  methods: {
    initTimeline() {
      var container = document.getElementById("visualization");
      this.data = new vis.DataSet();
      this.timeline = new vis.Timeline(
        container,
        this.data,
        this.timelineOptions
      );
    },
    move(percentage) {
      var range = this.timeline.getWindow();
      var interval = range.end - range.start;
      this.timeline.setWindow({
        start: range.start.valueOf() - interval * percentage,
        end: range.end.valueOf() - interval * percentage,
      });
    },
    moveLeft() {
      this.move(0.2);
    },
    moveRight() {
      this.move(-0.2);
    },
    resetTimeline() {
      var items = this.data;
      document.getElementById("sliderZoom").value = 0;
      this.timeline.fit(items.getIds());
    },
    addItemToTimeline(item) {
      console.log("itemReceived", item);
      if (item.toDelete) {
        console.log("remove", item.id);
        this.data.remove(item.id);
      } else {
        let dataToAdd = {
          id: item.id,
          content: item.name,
          start: `${item.date} ${item.hourStart}`,
          end: `${item.date} ${item.hourFinish}`,
        };
        this.data.add(dataToAdd);
      }
    },
    zoomTimeline() {
      var items = this.data;
      var value = this.value;
      var start = null;
      var end = null;
      if (value < 0) {
        (start = moment().year(moment().year() - 100000)), // to adjust with options
          (end = moment().year(moment().year() + 1));
        this.timeline.zoomOut(-value);
        if (value === "-1") this.timeline.setWindow(start, end);
      } else if (value > 0) {
        (start = moment()), (end = moment(moment().utc() + 10));
        this.timeline.zoomIn(value);
        if (value === "1") this.timeline.setWindow(start, end);
      } else {
        this.timeline.fit(items.getIds());
        this.value = 0;
      }
    },
  },
  watch: {
    item(value) {
      this.addItemToTimeline(value);
    },
  },
};
</script>

<style>
@import "./style.css";
@import "./full-style.css";
.arrow_right {
  height: 166px;
  width: 45px;
  border: 1px solid black;
}
</style>