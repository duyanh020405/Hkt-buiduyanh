<template>
    <div>
        <h1>Add User</h1>
        <form @submit.prevent="handleSubmit">
            <h3>Name:</h3>
            <input type="text" v-model="name" required />

            <h3>Gender:</h3>
            <p>
                <input type="radio" id="man" value="Male" v-model="selectedGender" required />
                <label for="man">Male</label>
            </p>
            <p>
                <input type="radio" id="woman" value="Female" v-model="selectedGender" required />
                <label for="woman">Female</label>
            </p>
            <p>
                <input type="radio" id="other" value="Other" v-model="selectedGender" required />
                <label for="other">Other</label>
            </p>

            <h3>Date of Birth:</h3>
            <input type="date" v-model="dateOfBirth" required />
            <span v-if="dateError" style="color: red;">{{ dateError }}</span>

            <h3>Email:</h3>
            <input type="email" v-model="email" required />
            <span v-if="emailError" style="color: red;">{{ emailError }}</span>

            <button type="submit">Submit</button>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
const router = useRouter()
const id = ref('')
const name = ref('');
const selectedGender = ref('');
const dateOfBirth = ref('');
const email = ref('');
const submitted = ref(false);
const emailError = ref('');
const dateError = ref('');

const validateEmail = (email) => {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email);
};

const validateDate = (date) => {
    const today = new Date();
    const selectedDate = new Date(date);
    return selectedDate < today;
};
const generateRandomID = () => {
    return Math.floor(Math.random() * 1000000).toString();
};

const handleSubmit = () => {
    emailError.value = '';
    dateError.value = '';

    if (!validateEmail(email.value)) {
        emailError.value = 'Vui long dung dinh dang email';
        return;
    }

    if (!validateDate(dateOfBirth.value)) {
        dateError.value = 'Ngay sinh khong hop le';
        return;
    }

    submitted.value = true;
    const userData = {
        id: generateRandomID(),
        name: name.value,
        selectedGender: selectedGender.value,
        dateOfBirth: dateOfBirth.value,
        email: email.value,
    };
    submitted.value = true;
    const allUsers = JSON.parse(localStorage.getItem('users')) || [];
    allUsers.push(userData);
    localStorage.setItem('users', JSON.stringify(allUsers));
    alert("Them thanh cong")
    name.value = ''
    selectedGender.value = ''
    dateOfBirth.value = ''
    email.value = ''
    router.push('/admin/manager-user')
};
</script>

<style scoped>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 20px;
}

div {
    max-width: 400px;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
    color: #333;
}

h3 {
    margin-bottom: 10px;
}

input[type="text"],
input[type="date"],
input[type="email"],
button {
    width: 100%;
    padding: 10px;
    margin: 5px 0 20px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

p {
    margin: 10px 0;
}

button {
    background-color: #28a745;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}

h3 {
    margin-top: 20px;
}
</style>
