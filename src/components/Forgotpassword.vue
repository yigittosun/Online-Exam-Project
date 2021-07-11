<!-- Forgotpassword Page-->
<template>
    <div class="vue-tempalte">
        <form>
            <h3>Forgot Password</h3>

            <div class="form-group">
                <label>Email Address</label>
                <input type="email" placeholder="Enter your e-mail" class="form-control form-control-lg" />
                <label>Code</label>
                <input type="email" placeholder="Enter the code sent to your e-mail" class="form-control form-control-lg" />
            </div>
            <br>
            <router-link type="submit" class="btn btn-dark btn-lg btn-block" to="/login" >Reset Password</router-link>

        </form>
    </div>
</template>

<script>
    export default {
        data() {
            return {}
        }
    }
</script>

<style>
input,
input::-webkit-input-placeholder {
    font-size: 15px;
    line-height: 3;
}

</style>