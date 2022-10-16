<template>
  <base-dialog v-if="inputIsInvalid" title="Invalid Input!">
    <template #default>
      <p>Sorry, input can't be empty🥺</p>
      <p>
        check each input and make sure no input is empty, input more details...
      </p>
    </template>
    <template #actions>
      <button @click="confirmError">Okay</button>
    </template>
  </base-dialog>
  <base-card>
    <form @submit.prevent="submitResource">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text" id="title" name="title" ref="titleInput" />
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea
          name="description"
          id="description"
          rows="3"
          ref="descInput"
        ></textarea>
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input type="url" id="link" name="link" ref="urlInput" />
      </div>
      <p v-if="error">{{ error }}</p>
      <div>
        <base-button type="submit">Add Resource</base-button>
      </div>
    </form>
  </base-card>
</template>

<script>
export default {
  emits: ["changeTab"],
  data() {
    return {
      inputIsInvalid: false,
      error: null,
      storedResources: this.resources,
    };
  },
  // computed: {
  //   storedResources() {
  //     return this.resources;
  //   },
  // },
  methods: {
    submitResource() {
      const enteredTitle = this.$refs.titleInput.value;
      const enteredDesc = this.$refs.descInput.value;
      const enteredUrl = this.$refs.urlInput.value;

      if (
        enteredTitle.trim() === "" ||
        enteredDesc.trim() === "" ||
        enteredUrl.trim() === ""
      ) {
        this.inputIsInvalid = true;
      } else {
        // this.addResource(enteredTitle, enteredDesc, enteredUrl);
        fetch(
          "https://http-demo-b02d8-default-rtdb.asia-southeast1.firebasedatabase.app/resources.json",
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              title: enteredTitle,
              description: enteredDesc,
              url: enteredUrl,
            }),
          }
        )
          .then((res) => {
            if (res.ok) {
              this.$emit("changeTab", enteredTitle, enteredDesc, enteredUrl);
            } else {
              throw new Error("Could not save data:(");
            }
          })
          .catch((err) => {
            console.log(err);
            this.error = err.message;
          });
      }
    },
    confirmError() {
      this.inputIsInvalid = false;
    },
  },
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}
input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 2px solid #ccc;
  padding: 0.5em;
}
input:focus,
textarea:focus {
  outline: none;
  border-color: gray;
}
.form-control {
  margin: 1rem 0 1.6rem;
}

button {
  background-color: rgba(51, 51, 51, 0.05);
  border-radius: 8px;
  border-width: 0;
  color: #333333;
  cursor: pointer;
  display: inline-block;
  font-family: "Haas Grot Text R Web", "Helvetica Neue", Helvetica, Arial,
    sans-serif;
  font-size: 14px;
  font-weight: 500;
  line-height: 20px;
  list-style: none;
  margin: 0;
  padding: 10px 12px;
  text-align: center;
  transition: all 200ms;
  vertical-align: baseline;
  white-space: nowrap;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}
</style>
