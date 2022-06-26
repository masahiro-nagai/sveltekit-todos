<script>
    import {goto} from "$app/navigation";
    import supabase from "$lib/db";
    import {user} from "$lib/stores";
    let email = "";
    const signUp = async() =>{
        let { user: userDetails, error } = await supabase.auth.signUp({
            email: email,
            password: 'ylUhwmaHBuOtqVZTFIxE'
        })
        $user = userDetails;
        goto("/");
    }
    const logIn = async() => {
        let { user: userDetails, error } = await supabase.auth.signIn({
            email: email,
            password: 'ylUhwmaHBuOtqVZTFIxE'
        })
        $user = userDetails;
        goto("/");
    }
</script>

<label for="">
    Email:
    <input bind:value={email} type="email" placeholder="email@email.com">
</label>
<br>
<br>
<button on:click={signUp}>signup</button>
<button on:click={logIn}>Login</button>
<style>

</style>