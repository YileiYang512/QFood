<template>
    <div class="admin-container">
        <div class="admin-form-container">
            <form id="adminForm" @submit="handleSubmit" novalidate autocomplete="off">
                <h3>LOGIN</h3>

                <div v-if="errors.length" class="error-box">
                    <ul>
                        <li v-for="error in errors" :key="error">{{ error }}</li>
                    </ul>
                </div>

                <div class="form-group">
                    <input type="password" id="uPass" name="uPass" class="form-control"
                        placeholder="Enter password" v-model="loginObj.password" />
                </div>

                <div class="form-group">
                    <label for="userType">Login As:</label>
                    <select id="userType" v-model="loginObj.userType" class="form-control">
                        <option value="admin">Admin</option>
                        <option value="waiter">Waiter</option>
                    </select>
                </div>

                <div class="form-group">
                    <input type="submit" value="Login" class="btn">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import { mapMutations } from "vuex";

export default {
    name: 'Admin',

    data() {
        return {
            loginObj: { password: "", userType: "admin" },
            adminKey: "Admin", // Admin password
            waiterKey: "Waiter", // Waiter password
            errors: [],
        }
    },

    methods: {
        ...mapMutations(["setAdmin"]),

        handleSubmit(e) {
            this.errors = [];
            e.preventDefault();

            if (!this.loginObj.password) {
                this.errors.push('Password is required');
                return;
            }

            if (this.loginObj.userType === 'admin') {
                // Login as admin
                if (this.loginObj.password === this.adminKey) {
                    this.setAdmin("admin");
                    this.$router.push("/admin/dashboard");
                } else {
                    this.errors.push("Admin password is wrong!")
                }
            } else {
               // Login as admin
                if (this.loginObj.password === this.waiterKey) {
                    this.setAdmin("admin");
                    this.$router.push("/admin/Orders");
                } else {
                    this.errors.push("Admin password is wrong!")
                }
            }
        }
    }
}
</script>



<style scoped>
.admin-container {
    padding: 2rem 9%;
}

.admin-container .admin-form-container {
    background-color: #fff;
    height: 100vh;
}

.admin-container .admin-form-container form {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    max-width: 40rem;
    width: 100%;
    box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.05);
    border: 0.1rem solid rgba(0, 0, 0, 0.2);
    padding: 2rem;
    border-radius: .5rem;
    animation: fadeUp .4s linear;
}

.admin-container .admin-form-container form h3 {
    padding-bottom: 1rem;
    font-size: 2rem;
    font-weight: bolder;
    text-transform: uppercase;
    color: #130f40;
    margin: 0;
}

.admin-container .admin-form-container form .form-control {
    margin: .7rem 0;
    border-radius: .5rem;
    background: #f7f7f7;
    padding: 2rem 1.2rem;
    font-size: 1.6rem;
    color: #130f40;
    text-transform: none;
    width: 100%;
    border: none;
}

.admin-container .admin-form-container form .btn {
    margin-bottom: 1rem;
    margin-top: 1rem;
    width: 100%;
}

.admin-container .admin-form-container form p {
    padding-top: 1rem;
    font-size: 1.5rem;
    color: #666;
    margin: 0;
}

.admin-container .admin-form-container form p a {
    color: #27ae60;
}

.admin-container .admin-form-container form p a:hover {
    color: #130f40;
    text-decoration: underline;
}

.admin-container .admin-form-container form .error-box {
    background-color: #fff9fa;
    box-sizing: border-box;
    border: 2px solid rgba(255, 66, 79, .2);
    border-radius: 2px;
    font-size: 12px;
    margin-bottom: 20px;
}

.admin-container .admin-form-container form .error-box ul {
    list-style-type: none;
    margin: 0;
    padding: 10px 0px;
}

.admin-container .admin-form-container form .error-box ul li {
    padding-left: 10px;
    color: rgb(182, 0, 0);
}
</style>