---
title: svelte.js 登录示例代码
tags:
- svelte
- code
- auth
categories:
date: 2023-02-07
lastMod: 2023-02-07
---


```javascript
<script>
    import { onMount } from 'svelte';
    import { db } from './pocketbase.js';
    
    let username = '';
    let password = '';
    let error = '';
    
    onMount(async () => {
        try {
            await db.open();
        } catch (e) {
            console.error(e);
        }
    });
    
    async function handleSubmit() {
        try {
            const user = await db.getUser(username);
            if (!user) {
                error = '用户名不存在';
                return;
            }
            if (user.password !== password) {
                error = '密码不正确';
                return;
            }
            // 登录成功的代码
            error = '';
        } catch (e) {
            console.error(e);
            error = '登录失败，请稍后重试';
        }
    }
</script>

<form on:submit|preventDefault={handleSubmit}>
    <label>
        用户名：
        <input type="text" bind:value={username} />
    </label>
    <br />
    <label>
        密码：
        <input type="password" bind:value={password} />
    </label>
    <br />
    {#if error}
        <p style="color: red">{error}</p>
    {/if}
    <button type="submit">登录</button>
</form>

```
