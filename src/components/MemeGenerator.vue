<template>
  <div class="bg-gray-800 p-5 h-[100%]">
    <span className="z-10">
        <a href="http://instagram.com/dev_1.1.1.1">
          <img
            className="absolute top-5 right-5 h-16 w-16"
            src="https://img.icons8.com/clouds/50/instagram-new--v3.png"
            alt=""
          />
        </a>
      </span>

      <h1 className="mt-8 text-white text-4xl font-bold text-center p-8 flex w-full justify-center items-center">
        <img
          width="50"
          height="50"
          src="https://img.icons8.com/3d-fluency/50/face-with-tears-of-joy-icon.png"
          alt="face-with-tears-of-joy-icon"
        />
        &nbsp; Meme Generator &nbsp;
        <img
          width="50"
          height="50"
          src="https://img.icons8.com/3d-fluency/50/face-with-tears-of-joy-icon.png"
          alt="face-with-tears-of-joy-icon"
        />
      </h1>

    <div class="upload mb-5 mt-8 p-4 w-full">
      <h2 class="text-white text-3xl font-bold text-center">Upload Images</h2>
      <div class="flex justify-center items-center p-4 m-4">
        <input
          type="file"
          id="imageUpload"
          accept="image/*"
          multiple
          @change="handleImageUpload"
        />
      </div>
    </div>

    <div class="images flex justify-center items-center gap-4 mb-10">
      <img
        v-for="image in images"
        :key="image.id"
        :src="image.src"
        :alt="'Uploaded Image'"
        :id="image.id"
        class="draggable"
        @dragstart="handleDragStart($event, image)"
      />
    </div>

    <div class="custom-category mb-5 w-full">
      <h2 class="text-white text-3xl font-bold text-center">
        Custom Categories
      </h2>
      <div class="flex p-4 gap-2 justify-center items-center">
        <input
          type="text"
          id="categoryName"
          placeholder="Enter Category Name"
          class="text-sm p-2 border rounded"
          v-model="newCategoryName"
        />
        <input
          type="color"
          id="categoryColor"
          class="border rounded"
          v-model="newCategoryColor"
        />
        <button
          class="bg-violet-500 text-white p-2 rounded hover:bg-violet-400"
          @click="handleAddCategory"
        >
          Add Category
        </button>
      </div>
    </div>

    <div class="categories space-y-4 mt-10 p-8">
      <div
        v-for="(category, index) in categories"
        :key="index"
        class="category drop-zone flex flex-row space-x-3 p-2 overflow-auto rounded-2xl"
        :style="{ backgroundColor: category.color }"
        @dragover="handleDragOver"
        @dragleave="handleDragLeave"
        @drop="handleDrop"
      >
        <button
          className="bg-transparent text-white rounded"
          @click="handleDeleteCategory(index)"
        >
          <img
            className="h-12 w-12 rounded-2xl"
            src="https://bg-so-1.zippyimage.com/2024/05/05/8aa02ba68b7a711628d58064595e5ed3.gif"
            alt="deleteBinImg"
          />
        </button>
        <h2 class="text-xl font-bold p-2 flex justify-center items-center">{{ category.name }}</h2>
      </div>
    </div>

    <!-- <button
      class="bg-red-500 text-white p-2 rounded hover:bg-red-400"
      @click="handleDeleteCategory()"
    >
      Delete
    </button> -->
  </div>
</template>

<script>
export default {
  name: "MemeGenerator",
  data() {
    return {
      categories: [],
      images: [],
      newCategoryName: "",
      newCategoryColor: "#000000",
    };
  },
  methods: {
    handleAddCategory() {
      if (this.newCategoryName.trim() === "") {
        window.alert("Please enter a category name");
        return;
      }

      const newCategory = {
        name: this.newCategoryName,
        color: this.newCategoryColor,
      };

      this.categories.push(newCategory);
      this.newCategoryName = "";
      this.newCategoryColor = "#000000";
    },
    handleImageUpload(event) {
      const files = event.target.files;

      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        const reader = new FileReader();

        reader.onload = (event) => {
          const newImage = {
            src: event.target.result,
            id: `uploadedImage${this.images.length + i}`,
          };

          this.images.push(newImage);
        };

        reader.readAsDataURL(file);
      }
    },
    handleDragStart(event, image) {
      event.dataTransfer.setData("text", image.id);
    },
    handleDragOver(event) {
      event.preventDefault();
      event.currentTarget.classList.add("bg-gray-400");
    },
    handleDragLeave(event) {
      event.preventDefault();
      event.currentTarget.classList.remove("bg-gray-400");
    },
    handleDrop(event) {
      event.preventDefault();
      event.currentTarget.classList.remove("bg-gray-400");
      const id = event.dataTransfer.getData("text");
      const draggableElement = document.getElementById(id);
      event.currentTarget.appendChild(draggableElement);
    },
    handleDeleteCategory(index) {
      if (typeof index !== "undefined") {
        this.categories.splice(index, 1);
      } else {
        this.categories = [];
      }
    },
  },
};
</script>
