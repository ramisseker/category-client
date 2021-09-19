<template>
  <div class="columns ">
    <section class="column is-one-quarter">
      <div>
        <div class="columns is-justify-content-space-between m-2">
          <div>
            <h1>Categories</h1>
          </div>
          <div>
            <input
              class="mr-3"
              type="text"
              :placeholder="category.category_name"
              v-model="categoryName"
            />
            <button
              class="button is-warning is-small"
              @click="updateCategory(category.id)"
            >
              Edit
            </button>
            <button
              class="button is-danger is-small"
              @click="deleteCategory(category.id)"
            >
              Delete
            </button>
            <button class="button is-info is-small" @click="saveCategory">
              Add
            </button>
          </div>
        </div>
        <div class="categories">
          <ul>
            <li
              class="listitems is-12"
              v-for="item in items"
              :key="item.id"
              @click="
                subCategoriesById(item.id);
                getCategoriesById(item.id);
              "
            >
              {{ item.category_name }}
            </li>
          </ul>
        </div>
      </div>
    </section>
    <section class="column is-one-quarter">
      <div>
        <div
          class="columns  is-one-quarteris-justify-content-space-between m-2"
        >
          <div>
            <h1>SubCategories</h1>
          </div>
          <div>
            <input
              class="mr-3"
              type="text"
              placeholder="Sub Category Name"
              v-model="subCategoryName"
            />
            <button
              class="button is-warning is-small"
              @click="updateSubCategory(subItems[0].id)"
            >
              Edit
            </button>
            <button
              class="button is-danger is-small"
              @click="deleteSubCategory(subItems[0].id)"
            >
              Delete
            </button>
            <button
              class="button is-info is-small"
              @click="saveSubCategory(category.id)"
            >
              Add
            </button>
          </div>
        </div>
        <div class="categories">
          <ul>
            <li
              class="listitems is-12"
              v-for="item in subItems"
              :key="item.id"
              @click="subCategoriesById2(item.id)"
            >
              {{ item.subcatname }}
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CategoryList',
  data() {
    return {
      categoryName: '',
      subCategoryName: '',
      category_id: '',
      subCategoryName2: '',
      subcat_id: '',
      items: [{ category_name: 'Category Ekleyiniz' }],
      subItems: [{ subcatname: 'Category Ekleyiniz' }],
      subItems2: [],
      itemId: '0',
      category: { category_name: ' Category' },
      disabled: 0,
    };
  },

  created() {
    this.getCategory();
  },

  methods: {
    async getCategory() {
      try {
        const response = await axios.get(
          'https://e-commerence-category.herokuapp.com/api/categories'
        );
        this.items = response.data;
      } catch (err) {
        console.log(err);
      }
    },
    async getCategoriesById(id) {
      try {
        const response = await axios.get(
          `https://e-commerence-category.herokuapp.com/api/categories/${id}`
        );
        this.category = response.data;
        console.log(this.category);
      } catch (err) {
        console.log(err);
      }
    },

    async saveCategory() {
      try {
        await axios.post(
          'https://e-commerence-category.herokuapp.com/api/categories',
          {
            category_name: this.categoryName,
          }
        );
        this.categoryName = '';
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    async updateCategory(id) {
      try {
        await axios.put(
          `https://e-commerence-category.herokuapp.com/api/categories/${id}`,
          {
            category_name: this.categoryName,
          }
        );
        this.categoryName = '';
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    async updateSubCategory(id) {
      try {
        await axios.put(
          `https://e-commerence-category.herokuapp.com/api/subcategories/${id}`,
          {
            category_name: this.subCategoryName,
          }
        );
        this.subCategoryName = '';
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    async subCategoriesById(id) {
      try {
        const response = await axios.get(
          `https://e-commerence-category.herokuapp.com/api/subcategories/${id}`
        );
        this.subItems = response.data;
        console.log(response.data);
      } catch (err) {
        console.log(err);
      }
    },
    // async subCategoriesById(id, category_name) {
    //   try {
    //     await axios.post(`https://e-commerence-category.herokuapp.com/api/${category_name}`, {
    //       category_name: category_name,
    //     });
    //     alert('Category oluşturuldu.');

    //     const response = await axios.get(
    //       `https://e-commerence-category.herokuapp.com/api/${category_name}/${id}`
    //     );
    //     this.items = response.data;
    //   } catch (err) {
    //     console.log(err);
    //   }
    // },
    async subCategoriesById2(id) {
      try {
        const response = await axios.get(
          `https://e-commerence-category.herokuapp.com/api/subcategories2/${id}`
        );
        this.subItems2 = response.data;
      } catch (err) {
        console.log(err);
      }
    },
    async saveSubCategory(id) {
      try {
        await axios.post(
          'https://e-commerence-category.herokuapp.com/api/subcategories',
          {
            subcatname: this.subCategoryName,
            category_id: id,
          }
        );
        this.subCategoryName = '';
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },

    async saveSubCategory2() {
      try {
        await axios.post(
          'https://e-commerence-category.herokuapp.com/api/subcategories2',
          {
            subcatname2: this.subCategoryName2,
            subcat_id: this.subItems2[0].subcat_id,
          }
        );
        this.subCategoryName2 = '';
        this.subcat_id = '';
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    async deleteCategory(id) {
      console.log('id');
      console.log(id);
      try {
        await axios.delete(
          `https://e-commerence-category.herokuapp.com/api/categories/${id}`
        );
        this.getCategory();
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    async deleteSubCategory(id) {
      console.log('id');
      console.log(id);
      try {
        await axios.delete(
          `https://e-commerence-category.herokuapp.com/api/subcategories/${id}`
        );
        this.$router.go('/');
      } catch (err) {
        console.log(err);
      }
    },
    basildi() {
      alert('basıldı...');
    },
    visibility() {},
  },
};
</script>

<style scoped>
.categories {
  height: 20rem;
  border: 2px solid #000000;
}
.listitems {
  border: 1px solid #000000;
}

.nav p {
  display: none;
}
</style>
