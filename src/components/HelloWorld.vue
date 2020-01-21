<template>
  <div>
    <textarea v-model="text" id="txt" ref="txt"></textarea>
    <div class="freinds" v-if="isArobase">
      <input type="text" v-model="search">
      <ul>
        <li v-for="fr in freindsList" @click="addToTextArea(fr)">{{fr.name}}</li>
      </ul>
    </div>
    <span v-for="f in commentUsers">
      <a href="#">{{f.name}}</a>,
    </span>

    {{commentUsers}}
  </div>
</template>


<script>

export default {
  data() {
    return {
      search: "",
      text: "",
      isArobase: false,
      freinds: [
        { id: 1, name: "fr1" },
        { id: 2, name: "fr2" },
        { id: 3, name: "fr3" },
        { id: 4, name: "fr4" },
        { id: 5, name: "fr5" },
        { id: 6, name: "fr6" }
      ],
      commentUsers: []
    };
  },
  methods: {
    addToTextArea(fr) {
      var input = this.$refs.txt;
      let selectionStart = input.selectionStart;

      let start = input.selectionStart;
      console.log("start " + start);

      var ch =
        this.text.substr(0, selectionStart) +
        fr.name +
        " " +
        this.text.substr(selectionStart);
      let end = input.selectionStart + fr.name.length;
      console.log("end " + end);
      this.text = ch;
      this.isArobase = true;
      input.focus();
      this.search = "";
      this.commentUsers.push({
        id: fr.id,
        name: fr.name,
        start: start,
        end: end - 1
      });
    }
  },
  watch: {
    text(nv, old) {
      var input = this.$refs.txt;

      if (old.substr(input.selectionStart - 1, 1) == "@") {
        this.isArobase = false;
      } else {
        let u = this.commentUsers.find(
          user => input.value.indexOf('@' + user.name) ==-1
            
        );
        console.log(u);
        if (u) {
          this.commentUsers = this.commentUsers.filter(user => user.id != u.id);
        }
        console.log(input.selectionStart, " ", nv, " ", old);
        if (input.value.substr(input.selectionStart - 1, 1) == "@") {
          this.isArobase = true;
          // this.currentPos = input.selectionStart
        } else {
          this.isArobase = false;
        }
      }
    }
  },
  computed: {
    freindsList() {
      if (this.search != "") {
        return this.freinds.filter(item => item.indexOf(this.search) != -1);
      } else {
        return this.freinds;
      }
    }
  },
  mounted() {
    console.log(this.$refs);
  }
};
</script>


<style scoped>
.freinds {
  background: #eee;
  position: absolute;
  top: 35px;
}

ul {
  list-style: none;
  background: #fff;
  padding: 0;
  margin: 0;
}

li {
  background: #eee;
  padding: 5px 4px;
  cursor: pointer;
  margin: 1px 0;
  transition: background 0.5s ease;
}

li:hover {
  background: #fff;
}
</style>
