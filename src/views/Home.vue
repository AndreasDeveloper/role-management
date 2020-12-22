<template>
  <div class="home">
    <h1>User Roles Management</h1>
    <TopNav @initSearch="search" @initFilter="filter" />
    <div class="role-wrapper">
      <RoleCard v-for="(role, i) in data" :key="i" :passedData="role" :id="role.id" @initDelete="deleteRole" @initEdit="editRole" />
    </div>
  </div>
</template>

<script>
import TopNav from '@/components/base/TopNav';
import RoleCard from '@/components/home/RoleCard';
import { mapState } from 'vuex';
// import seedData from '@/assets/user_roles.json'; -- Import directly from local JSON file - example

export default {
  name: 'Home',
  components: {
    TopNav,
    RoleCard
  },
  data() {
    return {
      // data: seedData -- Local JSON file import
      data: null
    }
  },
  computed: {
    ...mapState({
      roles: state => state.roles // Using state to fetch data from the store
    })
  },
  mounted() {
    this.data = this.roles;
  },
  methods: {
    search(input) { // Search role functionality
      const clonedData = [...this.data];
      let emptyArr = [];
      if (!input) {
        this.data = this.roles;
      } else {
        let re = new RegExp(input + '.*$', 'i');
        emptyArr = clonedData.filter(function(e, i, a) {
          if (e.name !== null) {
            return e.name.search(re) != -1;
          }
        });
        this.data = emptyArr;
      }
    },
    filter(rule) { // Filter role functionality (Active, inactive, both)
      this.data = this.roles;
      let clonedData = [...this.data];
      let emptyArr = [];

      clonedData.map(el => {
        if (rule == 1 && !el.active) { // Show only active roles
          clonedData.splice(clonedData.indexOf(el), 1);
          this.data = clonedData;
        }
        if (rule == 2 && !el.active) { // Show only inactive roles
          this.data = [];
          emptyArr.push(el);
          this.data = emptyArr;
        }
        if (rule == 3) { // Show both active & inactive roles
          clonedData = this.roles;
          this.data = clonedData;
        }
      });
    },
    deleteRole(role) { // Delete role functionality
      let clonedData = [...this.data];

      clonedData.map(el => {
        if (el.id === role.id) {
          clonedData.splice(clonedData.indexOf(el), 1);
          this.data = clonedData;
        }
      });
    },
    editRole(role) {
      this.$router.push({
        path: '/role',
        name: 'Role',
        params: {
          role
        }
      });
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  margin: 40px 70px;
  h1 {
    font-weight: 300;
    margin-bottom: 80px;
    text-align: left;
  }
  .role-wrapper {
    display: flex;
    flex-wrap: wrap;
    margin-top: 40px;
  }
}
</style>