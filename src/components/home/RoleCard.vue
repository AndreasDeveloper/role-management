<template>
    <div class="wrap">
        <div class="role-card">
            <div class="role-card__status" v-if="!role.active">
                <span>Inactive</span>
            </div>
            <div class="role-card__main">
                <h3>{{ role.name }}</h3>
                <h4>{{ role.type }}</h4>
            </div>
            <p>{{ role.description.substring(0, 85) }}...</p>
            <div class="role-card__users">
                <img v-for="(user, i) in role.users" :key="i" :src="user.photo_url" alt="User" />
            </div>
        </div>
        <div class="role-card-bottom">
            <div class="role-card-bottom__date">
                <span>Date created: {{ role.created_on.substring(0, 10).split('-').join('/') }}</span>
            </div>
            <div class="role-card-bottom__lock" v-if="!role.editable">
                <ion-icon name="lock-closed-outline"></ion-icon>
            </div>
            <div class="role-card-bottom__buttons" v-else>
                <button class="role-card-bottom__buttons--edit">Edit</button>
                <button class="role-card-bottom__buttons--delete" @click="initDelete">Delete</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'RoleCard',
    data() {
        return {
            role: {}
        }
    },
    props: {
        passedData: {
            type: Object
        }
    },
    watch: {
        passedData: {
            deep: true,
            immediate: true,
            handler(v) {
                this.role = v;
            }
        }
    },
    methods: {
        initDelete() {
            this.$emit('initDelete', this.role);
        }
    }
}
</script>

<style lang="scss" scoped>
%flexCenter {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.wrap {
    border: 1px solid #cfcfcf;
    margin: 10px 20px;
    text-align: left;
    width: 30%;
}

.role-card {
    position: relative;
    padding: 50px 25px 0 25px;
    &__main {
        h4 {
            font-weight: 300;
        }
    }
    p { 
        font-size: 14px; 
        font-weight: 300;
    }
    &__users {
        margin-top: 25px;
        img {
            border-radius: 50%;
            margin-right: 5px;
            height: 40px;
            width: 40px;
        }
    }
    &__status {
        background-color: #e74c3c;
        border-radius: 5px;
        color: #fff;
        font-size: 14px;
        text-transform: uppercase;
        position: absolute;
        padding: 5px;
        right: 5%;
        top: 10%;
    }
}

.role-card-bottom {
    background: #fafafa;
    padding: 0 25px;
    margin-top: 25px;
    height: 50px;
    @extend %flexCenter;
    &__date {
        color: #adadad;
        font-size: 14px;
    }
    &__buttons {
        button {
            background: transparent;
            text-transform: uppercase;
        }
        &--delete {
            color: #e74c3c;
        }
    }
}
</style>