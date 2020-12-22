<template>
    <div class="top-nav">
        <div class="top-nav__fields">
            <!-- Search Field -->
            <div>
                <input class="top-nav__fields__search" type="text" placeholder="search" @input="search" v-model="searchInput" />
                <ion-icon class="top-nav__fields__search-icon" name="search-outline"></ion-icon>
            </div>

            <!-- Selection Field -->
            <div>
                <label for="roles">Role Status</label>
                <select id="roles" v-model="filter" @change="filterIt">
                    <option value="1">Active</option>
                    <option value="2">Inactive</option>
                    <option value="3">Active and Inactive</option>
                </select>
            </div>
        </div>
        <div>
            <router-link to="/role"><button class="create-cta">Create New Role</button></router-link>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TopNav',
    data() {
        return {
            filter: 3,
            searchInput: null
        }
    },
    methods: {
        search() {
            this.searchInput = this.searchInput.toLowerCase();
            this.$emit('initSearch', this.searchInput);
        },
        filterIt() {
            this.$emit('initFilter', this.filter);
        }
    }
}
</script>

<style lang="scss" scoped>
// Extends
%flexRow {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
%flexCol {
    display: flex;
    flex-direction: column;
}

.top-nav {
    @extend %flexRow;
    @media only screen and (max-width: 800px) {
        display: flex;
        flex-direction: column;
    }
    &__fields {
        text-align: left;
        width: 80%;
        &__search {
            &::placeholder {
                font-weight: 300;
            }
        }
        &__search-icon {
            position: absolute;
            right: -2%;
            @media only screen and (max-width: 800px) {
                right: 10%;
            }
        }
        & > :nth-child(1) {
            margin-left: 0;
        }
        & > :nth-child(2) {
            label {
                position: absolute;
                top: -100%;
                font-size: 12px;
                font-weight: 500;
            }
        }
        & > * {
            display: initial;
            position: relative;
            margin: 0 15px;
        }
        @media only screen and (max-width: 800px) {
            width: 100%;
            & > * {
                display: block;
                margin: 0;
                margin-bottom: 50px;
                width: 100%;
            }
        }
    }
}
</style>