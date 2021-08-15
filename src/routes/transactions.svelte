<script context="module">
    export async function preload({params}, {token}) {
        if (!token) {
            this.redirect(302, `/login`);
        }
    }
</script>

<script>
import {post} from 'utils.js';
import {goto} from '@sapper/app';

let accountFrom = '';
let accountTo = '';
let amount = '';
let explanation = '';
let error = null;

async function submit(event) {
    const response = await post(`auth/makeTransaction`, {accountFrom, accountTo, amount, explanation});
    error = response.error;
    if (!error) {
        goto('/overview');
    }
}
</script>

<svelte:head>
    <title>Transaction • barBank</title>
</svelte:head>

<div class="auth-page">
    <div class="container page">
        <div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Make a transaction</h1>
                {#if error}
                    <div class="alert alert-danger" role="alert">{error}</div>
                {/if}
                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group mt-2 mb-2">
                        <input class="form-control form-control-lg" type="text" required placeholder="Account from"
                               bind:value={accountFrom}>
                    </fieldset>
                    <fieldset class="form-group mt-2 mb-2">
                        <input class="form-control form-control-lg" type="text" required placeholder="Account to"
                               bind:value={accountTo}>
                    </fieldset>
                    <fieldset class="form-group mt-2 mb-2">
                        <input class="form-control form-control-lg" type="number" required placeholder="Amount"
                              bind:value={amount}>
                    </fieldset>
                    <fieldset class="form-group mt-2 mb-2">
                        <input class="form-control form-control-lg" type="text" required placeholder="Explanation"
                                bind:value={explanation}>
                    </fieldset>
                    <button class="btn btn-lg btn-primary pull-xs-right" type="submit">
                        Send
                    </button>
                </form>
            </div>
        </div>
    </div>
</div>

