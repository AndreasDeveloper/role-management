<template>
  <div class="home">
    <h1>User Roles Management</h1>
    <TopNav @initSearch="search" @initFilter="filter" />
    <div class="role-wrapper">
      <RoleCard v-for="(role, i) in data" :key="i" :passedData="role" :id="role.id" @initDelete="deleteRole" />
    </div>
  </div>
</template>

<script>
import TopNav from '@/components/base/TopNav';
import RoleCard from '@/components/home/RoleCard';
import { mapState } from 'vuex';
// import seedData from '@/assets/user_roles.json'; -- Import directly from local JSON file

export default {
  name: 'Home',
  components: {
    TopNav,
    RoleCard
  },
  data() {
    return {
      // data: seedData
      data: null
    }
  },
  computed: {
    ...mapState({
      roles: state => state.roles
    })
  },
  mounted() {
    this.data = this.roles;
  },
  methods: {
    search(input) {
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
    filter(rule) {
      this.data = this.roles;
      let clonedData = [...this.data];

      clonedData.map(el => {
        if (rule == 1 && !el.active) {
          clonedData.splice(clonedData.indexOf(el), 1);
          this.data = clonedData;
        }
        if (rule == 2 && !el.active) {
          this.data = [];
          this.data.push(el);
        }
        if (rule == 3) {
          clonedData = this.roles;
          this.data = clonedData;
        }
      });
    },
    deleteRole(role) {
      let clonedData = [...this.data];

      clonedData.map(el => {
        if (el.id === role.id) {
          clonedData.splice(clonedData.indexOf(el), 1);
          this.data = clonedData;
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