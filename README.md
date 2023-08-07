# gitalk-comments
notion-blog的免费gitalk博客留言板。

<img width="973" alt="スクリーンショット 2023-08-07 11 11 21" src="https://github.com/mooncat126/gitalk-comments/assets/112956463/943d8a74-5959-4fbf-944f-84047a6f6264">

# 步骤
为了在Nuxt.js中集成Gitalk博客留言板，您需要遵循以下步骤：

### 1. 安装所需的依赖

首先，你需要安装Gitalk：

```bash
npm install gitalk
```

### 2. 创建一个Gitalk组件

在你的Nuxt项目中，建立一个新的组件来容纳Gitalk。例如在 `components/GitalkComponent.vue`:

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
      clientID: '你的GitHub Application Client ID',
      clientSecret: '你的GitHub Application Client Secret',
      repo: '存储你的评论的GitHub仓库名称',
      owner: '仓库拥有者的GitHub用户名',
      admin: ['仓库管理员的GitHub用户名'],
      id: location.pathname,      // 用于区分不同文章的评论
      distractionFreeMode: false  // Facebook-like distraction free mode
    })
    
    gitalk.render('gitalk-container')
  }
}
</script>
```

### 3. 在页面中集成Gitalk组件

在你的Nuxt页面中，你可以简单地导入并使用Gitalk组件。例如，在 `pages/post/_id.vue`:

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

### 4. 设置GitHub应用

确保你在GitHub上创建了一个OAuth应用并将Client ID和Client Secret填入Gitalk的配置中。

1. 登录您的GitHub账号
首先，确保您已登录到GitHub。

2. 创建一个新的OAuth应用
在浏览器中，导航到：https://github.com/settings/applications/new 来创建一个新的OAuth应用。

填写必要的应用详情：

- `Application name`: 为您的应用命名。例如，可以命名为 "My Blog Gitalk Integration"。
- `Homepage URL`: 您博客的主页URL。
- `Application description`: 描述您的应用（可选）。
- `Authorization callback URL`: 这通常应设置为博客的URL。Gitalk在身份验证过程中会使用它。
- 点击"Register application"。


3. 获取Client ID和Client Secret
完成OAuth应用的注册后，您将被重定向到您的应用详情页面。在这里，您会看到Client ID和Client Secret。请确保妥善保存这些信息，但不要将Client Secret公之于众或存储在前端代码中。

4. 在Gitalk配置中使用Client ID和Client Secret
当您在Nuxt项目中配置Gitalk时，使用从上述步骤中获取的Client ID和Client Secret。如我之前所述，为了安全考虑，Client Secret不应直接暴露在前端代码中。而应考虑使用后端服务来完成身份验证的交互，或考虑使用其他不需要Client Secret的评论系统。

### 注意事项

- 为了安全，不要在前端代码中暴露你的`clientSecret`。考虑使用服务器中间件或其他后端服务来隐藏这个秘密。
- Gitalk使用GitHub issues作为评论。你所选择的仓库会存储所有的评论。
