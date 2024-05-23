<template>
  <div class="admin-container">
    <div class="d-flex justify-content-between">
      <h1>Hello Admin!</h1>
      <button class="btn" @click="handleLogout()">Logout</button>
    </div>

    <div class="table-responsive">
      <!-- FOOD TABLE -->
      <table class="table colored-header datatable project-list">
        <!-- Table headers -->
        <thead>
          <!-- Header row -->
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Star</th>
            <th>Vote</th>
            <th>Price</th>
            <th>Discount</th>
            <th>Description</th>
            <th>Status</th>
            <th>Type</th>
            <th>Category</th>
            <th>Source</th>
            <th>Action</th>
          </tr>
        </thead>
        <!-- Table body -->
        <tbody>
          <!-- Table rows -->
          <tr v-for="food in allFoods" :key="food.food_id">
            <td>{{ food.food_id }}</td>
            <td>{{ food.food_name }}</td>
            <td>{{ food.food_star }}</td>
            <td>{{ food.food_vote }}</td>
            <td>{{ food.food_price }}</td>
            <td>{{ food.food_discount }}</td>
            <td>{{ food.food_desc }}</td>
            <td>{{ food.food_status }}</td>
            <td>{{ food.food_type }}</td>
            <td>{{ food.food_category }}</td>
            <td>{{ food.food_src }}</td>
            <td>
              <button class="action-btn" @click="editFood(food)">
                Edit
              </button>
              <button class="cancel-btn" @click="deleteFood(food.food_id)">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div>
      <!-- Form for adding or editing food -->
      <h2>{{ editingFood ? 'Edit Food' : 'Add New Food' }}</h2>
      <form @submit.prevent="submitForm">
        <!-- Input fields for food attributes -->
        <input type="text" v-model="newFood.food_name" placeholder="Food Name" required />
        <input type="text" v-model="newFood.food_star" placeholder="Star" required />
        <input type="text" v-model="newFood.food_vote" placeholder="Vote" required />
        <input type="text" v-model="newFood.food_price" placeholder="Price" required />
        <input type="text" v-model="newFood.food_discount" placeholder="Discount" required />
        <input type="text" v-model="newFood.food_desc" placeholder="Description" required />
        <input type="text" v-model="newFood.food_status" placeholder="Status" required />
        <input type="text" v-model="newFood.food_type" placeholder="Type" required />
        <input type="text" v-model="newFood.food_category" placeholder="Category" required />
        <input type="text" v-model="newFood.food_src" placeholder="Source" required />
        <button type="submit">{{ editingFood ? 'Update' : 'Add' }}</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";

export default {
  name: "AdminDashboard",
  data() {
    return {
      newFood: {
        food_name: "",
        food_star: "",
        food_vote: "",
        food_price: "",
        food_discount: "",
        food_desc: "",
        food_status: "",
        food_type: "",
        food_category: "",
        food_src: ""
      },
      editingFood: null // Initially no food is being edited
    };
  },
  computed: {
    ...mapState(["allFoods"])
  },
  methods: {
    editFood(food) {
      // Set the editingFood to the selected food
      this.editingFood = food;
      // Populate form fields with the values of the selected food
      Object.assign(this.newFood, food);
    },
    async deleteFood(id) {
      try {
        await axios.delete(`/api/foods/${id}`);
        // Update the food list after deletion
        this.$store.dispatch("getAllFoods");
      } catch (error) {
        console.error("Error deleting food:", error);
      }
    },
    async submitForm() {
      try {
        if (this.editingFood) {
          // If editingFood is not null, update the existing food item
          await axios.put(`/api/foods/${this.editingFood.food_id}`, this.newFood);
          // Reset editingFood after update
          this.editingFood = null;
        } else {
          // If editingFood is null, add a new food item
          await axios.post("/api/foods", this.newFood);
        }
        // Clear the form after submission
        this.clearForm();
        // Update the food list after addition or update
        this.$store.dispatch("getAllFoods");
      } catch (error) {
        console.error("Error:", error);
      }
    },
    clearForm() {
      // Clear form fields
      Object.keys(this.newFood).forEach(key => this.newFood[key] = "");
    },
      handleLogout: function () {
            this.setAdmin("");
        }
  }
};
</script>



<style scoped>
.admin-container {
    background-color: #fff;
    height: 100vh;
    padding: 2rem 9%;
    font-size: 16px;
}

.table-responsive {
  margin-top: 8vh;
}

/* Form styling */
form {
  margin-top: 20px;
}

form input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

form button {
  background-color: #0da9ef;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
}

form button:hover {
  background-color: #27ae60;
}

/* Responsive styles */
@media screen and (max-width: 768px) {
  .admin-container {
    padding: 2rem 5%;
  }

  form input[type="text"] {
    width: 100%;
  }
}
.project-list>tbody>tr>td {
    padding: 12px 8px;
}

.project-list>tbody>tr>td .avatar {
    width: 22px;
    border: 1px solid #CCC;
}

.table-responsive {
    margin-top: 8vh;
}

.action-btn,
.cancel-btn,
.paid-btn {
    width: 100px;
    height: 25px;
    color: white;
    text-transform: capitalize;
}

.action-btn {
    background-color: #0da9ef;
    margin-right: 10px;
}

.cancel-btn,
.paid-btn {
    background-color: red;
}

.action-btn:hover {
    background-color: #27ae60;
}
</style>