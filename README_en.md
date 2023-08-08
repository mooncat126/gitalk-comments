[English](README_en.md) | [中文](README.md)

# gitalk-comments
Free gitalk comment board for notion-blog.

<img width="973" alt="Screenshot 2023-08-07 11 11 21" src="https://github.com/mooncat126/gitalk-comments/assets/112956463/943d8a74-5959-4fbf-944f-84047a6f6264">

# Steps
To integrate the Gitalk blog comment board in Nuxt.js, follow these steps:

### 1. Set Up GitHub Application

Ensure you've created an OAuth app on GitHub and filled in the Client ID and Client Secret into Gitalk's configuration.

1. Log into your GitHub account
First and foremost, ensure you're logged into GitHub.

2. Create a new OAuth app
In your browser, navigate to: https://github.com/settings/applications/new to create a new OAuth app.

  - Fill in the necessary app details:
    - `Application name`: Name your app, for example, "My Blog Gitalk Integration".
    - `Homepage URL`: The main URL of your blog.
    - `Application description`: Describe your app (optional).
    - `Authorization callback URL`: This should typically be set to the URL of your blog. Gitalk will use this during the authentication process.
  - Click "Register application".

3. Get the Client ID and Client Secret
After registering the OAuth app, you'll be redirected to your app's details page. Here, you'll find the Client ID and Client Secret. Make sure to securely save this info but never expose the Client Secret publicly or store it in frontend code.

4. Use the Client ID and Client Secret in Gitalk's Configuration
When you're configuring Gitalk in your Nuxt project, use the Client ID and Client Secret you got from the steps above.

### 2. Install Necessary Dependencies

First, you need to install Gitalk:

```bash
npm install gitalk
```

### 3. Create a Gitalk Component

In your Nuxt project, create a new component to house Gitalk. For instance, in `components/GitalkComponent.vue`:

```vue
<template>
  <div>
    <div id="gitalk-container"></div>
  </div>
</template>

<script>
import 'gitalk/dist/gitalk.css'
import Gitalk from 'gitalk'

export default {
  mounted() {
    const gitalk = new Gitalk({
      clientID: 'Your GitHub Application Client ID',
      clientSecret: 'Your GitHub Application Client Secret',
      repo: 'GitHub repository name where your comments will be stored',
      owner: 'GitHub username of the repository owner',
      admin: ['GitHub username(s) of the repository admin'],
      id: location.pathname,      // To differentiate comments between posts
      distractionFreeMode: false  // Facebook-like distraction free mode
    })
    
    gitalk.render('gitalk-container')
  }
}
</script>
```

### 4. Integrate the Gitalk Component in the Page

In your Nuxt page, you can simply import and utilize the Gitalk component. For instance, in `pages/post/_id.vue`:

```vue
<template>
  <div>
    <!-- Your post content here -->
    <GitalkComponent />
  </div>
</template>

<script>
import GitalkComponent from '~/components/GitalkComponent'

export default {
  components: {
    GitalkComponent
  }
}
</script>
```

### Considerations

- For security, never expose your `clientSecret` in the frontend code. Consider using env environment variables or other backend services to hide this secret.
- Gitalk uses GitHub issues for comments. The repository you choose will store all the comments.
