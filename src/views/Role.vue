<template>
    <div class="role">
        <div class="role__header">
            <router-link to="/"><ion-icon class="icon" name="arrow-back-circle-outline"></ion-icon></router-link>
            <h1>Add New Role</h1>
        </div>

        <div class="role-form">
            <h2>Insert Data</h2>
            <input class="role-form__field" type="text" placeholder="Role name" v-model="newRole.name" required>
            <input class="role-form__field" type="text" placeholder="Role type" v-model="newRole.type" required>
            <textarea class="role-form__field" placeholder="Role description" cols="2" rows="5" v-model="newRole.description" required></textarea>
            <div class="role-form__select-wrap role-form__select-wrap--1">
                <label for="state">State</label>
                <select id="state" class="role-form__field" v-model="newRole.active" required>
                    <option :value="true">Active</option>
                    <option :value="false">Inactive</option>
                </select>
            </div>

            <div class="role-form__select-wrap role-form__select-wrap--2">
                <label for="editable">Editable</label>
                <select id="editable" class="role-form__field" v-model="newRole.editable" required>
                    <option :value="true">True</option>
                    <option :value="false">False</option>
                </select>
            </div>
            <button v-if="!hasProps" class="create-cta" @click="passRole" :disabled="btnState">Create</button>
            <button v-else class="create-cta" @click="updateRole" :disabled="btnState">Update</button>
        </div>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';

export default {
    name: 'Role',
    data() {
        return {
            newRole: {
                id: null,
                name: '',
                type: null,
                description: null,
                active: true,
                created_on: null,
                editable: true
            },
            btnState: false,
            hasProps: false
        }
    },
    created() {
        if (this.$route.params.role) {
            this.newRole = this.$route.params.role;
            this.hasProps = true;
        }
    },
    computed: {
        ...mapState({
            roles: state => state.roles
        })
    },
    methods: {
        ...mapActions([
            'create',
            'update'
        ]),
        async passRole() {
            this.validateInput();
            this.btnState = true;
            this.newRole.id = this.roles.length + 1;
            this.newRole.created_on = new Date().toISOString().replace(/T.*/,'').split('-').reverse().join('-');
            try {
                window.setTimeout(() => { // Simulating API hit after 2 seconds
                    this.create(this.newRole); // Await here. But as it is static (seeded) data, not demonstrated.
                    this.btnState = false;
                }, 2000);
                window.alert('Successfully created new role');
                this.$router.push('/');
            } catch (err) {
                console.log(err);
            }
        },
        async updateRole() {
            this.validateInput();
            this.btnState = true;
            this.newRole.created_on = new Date().toISOString().replace(/T.*/,'').split('-').reverse().join('-');
            try {
                window.setTimeout(() => {
                    this.update(this.newRole); // Await here. But as it is static (seeded) data, not demonstrated.
                    this.btnState = false;
                }, 2000);
                window.alert('Successfully updated role');
                this.$router.push('/');
            } catch (err) {
                console.log(err);
            }
        },
        validateInput() {
            if (this.newRole.name === null && this.newRole.type === null && this.newRole.description === null) {
                window.alert('Fill up all of the fields!');
                return false;
            } else {
                return true;
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.role {
  margin: 40px 70px;
  &__header {
      display: flex;
      position: relative;
      h1 {
        font-weight: 300;
        margin-bottom: 80px;
        text-align: left;
    }
    .icon {
        position: absolute;
        left: -2%;
        top: 8%;
        font-size: 20px;
        cursor: pointer;
    }
  }
  .role-form {
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.3);
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 50%;
    margin: 0 auto;
    padding: 50px 30px;
    h2 {
        margin-bottom: 25px;
    }
    &__field {
        margin: 25px 0;
        width: 60%;
    }
    &__select-wrap {
        width: 100%;
        label {
            position: absolute;
        }
        &--1 {
            label {
                bottom: 23%;
            }
        }
        &--2 {
            label {
                bottom: 13%;
            }
        }
    }
  }
}
</style>
