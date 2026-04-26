# SBTI人格测试 Web版本

这是一个从微信小程序转换而来的Web版本，可以直接部署到GitHub Pages。

## 部署到GitHub Pages

1. 创建新的GitHub仓库
2. 将 `web` 文件夹内的所有内容推送到仓库的 `main` 分支
3. 进入仓库 Settings → Pages
4. Source 选择 `Deploy from a branch`，Branch 选择 `main`
5. 等待部署完成即可访问

## 项目结构

```
web/
├── index.html      # 首页
├── test.html       # 测试页
├── result.html     # 结果页
├── css/
│   └── style.css   # 样式文件
├── js/
│   └── app.js      # 核心逻辑和算法
└── images/
    └── sbti_images/  # 人格图片资源
```

## 功能说明

- 15道核心测试题 + 2道特殊题（饮酒Gate题目）
- 26种不同人格类型
- 随机题目顺序
- 本地存储答题进度
- 结果页面展示人格解析和15维度分析
- 支持保存人格图片