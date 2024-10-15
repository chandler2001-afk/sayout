<script>
    import { z } from "zod";

    const registrationSchema = z.object({
        name: z.string().min(1, "Name cannot be empty"),
        email: z.string().email({ message: "Kindly enter a valid email" }),
        username: z.string().min(1, 'Username cannot be empty'),
        ageConfirmed: z.boolean().refine((val) => val === true, {
            message: "You must confirm your age!"
        }),
        password: z.string().min(6, "Password must be at least 6 characters"),
        confirmPassword: z.string().min(6, "Confirm password cannot be empty")
    })
    .refine((data) => data.password === data.confirmPassword, {
    message: "Passwords do not match", // This is a string, which is correct
    path: ["confirmPassword"] // Indicates where the error is
})

    let formData = {
        email: '',
        name: '',
        username: '',
        ageConfirmed: false,
        password: '',
        confirmPassword: ''
    };
    const validate = () => {
    try {
        registrationSchema.parse(formData);
        return true;
    } catch (error) {
        if (error.errors) {
            error.errors.forEach(err => {
                alert(err.message);
            });
        }
        return false;
    }
};

const handleSubmit = (event) => {
    event.preventDefault();
    if (validate()) {
        alert("Registration Successful!");
    }
};
</script>

<main class="flex items-center justify-center min-h-screen bg-gray-100">
    <div class="bg-white shadow-md rounded-lg p-8 w-96">
        <div class="flex justify-center mb-4">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 2C6.48 2 2 6.48 2 12c0 5.52 4.48 10 10 10s10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm-1-13h2v6h-2zm0 8h2v2h-2z" />
            </svg>
        </div>
        <h2 class="text-center text-xl font-semibold text-gray-700">Register</h2>
        <form class="mt-4" on:submit={handleSubmit}>
            <div class="mb-4">
                <label class="block text-sm text-gray-600">Name</label>
                <input type="text" bind:value={formData.name} placeholder="Your Name" class="mt-1 block w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-300" required />
            </div>
            <div class="mb-4">
                <label class="block text-sm text-gray-600">Username</label>
                <input type="text" bind:value={formData.username} placeholder="Your Username" class="mt-1 block w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-300" required />
            </div>
            <div class="mb-4">
                <label class="block text-sm text-gray-600">Email</label>
                <input type="email" bind:value={formData.email} placeholder="you@example.com" class="mt-1 block w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-300" required />
            </div>
            <div class="mb-4">
                <label class="block text-sm text-gray-600">Password</label>
                <input type="password" bind:value={formData.password} placeholder="Your Password" class="mt-1 block w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-300" required />
            </div>
            <div class="mb-4">
                <label class="block text-sm text-gray-600">Confirm Password</label>
                <input type="password" bind:value={formData.confirmPassword} placeholder="Confirm Your Password" class="mt-1 block w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-300" required />
            </div>
            <div class="flex items-center mb-4">
                <input type="checkbox" bind:checked={formData.ageConfirmed} id="age-confirmed" class="mr-2 leading-tight" required />
                <label for="age-confirmed" class="text-sm text-gray-600">I am above 18 years old</label>
            </div>
            <button type="submit" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-md transition duration-200">Register</button>
        </form>
    </div>
</main>

<style>
    main {
        background-color: #f7fafc; /* Light gray background */
    }
</style>
