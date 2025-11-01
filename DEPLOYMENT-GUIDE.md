# Deployment Guide

This guide covers several options for deploying your blog to GitHub.

## Option 1: GitHub Pages with Jekyll (Recommended)

Jekyll is the native static site generator for GitHub Pages and offers the simplest deployment process.

### Quick Setup

1. **Create a new GitHub repository**
   ```bash
   # Initialize git in your backup directory
   cd C:\Users\jackd\Downloads\blog-backup
   git init
   git add .
   git commit -m "Initial blog backup"
   ```

2. **Create a GitHub repository**
   - Go to https://github.com/new
   - Name it `yourusername.github.io` (or any name you prefer)
   - Don't initialize with README (you already have one)

3. **Push to GitHub**
   ```bash
   git remote add origin https://github.com/yourusername/your-repo-name.git
   git branch -M main
   git push -u origin main
   ```

4. **Configure Jekyll**

   Create `_config.yml` in the root:
   ```yaml
   title: Another Tech Blog
   description: Technical insights on Microsoft Intune, Azure, and PowerShell
   author: Jack Davis
   email: cloudops@jackdavis.net
   url: "https://yourusername.github.io"
   baseurl: ""

   theme: minima  # or choose another theme

   # Build settings
   markdown: kramdown
   plugins:
     - jekyll-feed
     - jekyll-seo-tag
   ```

5. **Reorganize for Jekyll**
   ```bash
   # Move your blog post to _posts directory with date prefix
   mkdir _posts
   mv content/custom-reporting-intune.md _posts/2021-12-30-custom-reporting-intune.md

   # Move images to assets
   mkdir -p assets/images
   mv content/images/* assets/images/
   ```

6. **Update image paths in the markdown**

   Replace `./images/` with `/assets/images/` in your post.

7. **Enable GitHub Pages**
   - Go to repository Settings → Pages
   - Set source to "main" branch
   - Save and wait for deployment

### Popular Jekyll Themes

- **Minima** (default): Clean and simple
- **Minimal Mistakes**: Feature-rich, great for tech blogs
- **Jekyll Now**: Zero-configuration deployment
- **Chirpy**: Modern with dark mode support

## Option 2: Hugo

Hugo is faster than Jekyll and offers more flexibility.

### Setup

1. **Install Hugo**
   ```bash
   # Windows (using Chocolatey)
   choco install hugo-extended

   # Or download from https://gohugo.io/installation/
   ```

2. **Create a new Hugo site**
   ```bash
   hugo new site my-blog
   cd my-blog
   ```

3. **Add a theme**
   ```bash
   git init
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   ```

4. **Copy your content**
   ```bash
   # Copy your markdown file to content/posts/
   cp ../blog-backup/content/custom-reporting-intune.md content/posts/

   # Copy images to static/images/
   mkdir -p static/images
   cp -r ../blog-backup/content/images/* static/images/
   ```

5. **Configure Hugo** (`config.toml`):
   ```toml
   baseURL = 'https://yourusername.github.io/'
   languageCode = 'en-us'
   title = 'Another Tech Blog'
   theme = 'PaperMod'

   [params]
     author = 'Jack Davis'
     description = 'Technical insights on Microsoft Intune, Azure, and PowerShell'
   ```

6. **Deploy to GitHub Pages**
   ```bash
   # Build the site
   hugo

   # Create GitHub repository and push
   git add .
   git commit -m "Initial Hugo site"
   git remote add origin https://github.com/yourusername/your-repo.git
   git push -u origin main
   ```

7. **Set up GitHub Actions** (`.github/workflows/gh-pages.yml`):
   ```yaml
   name: Deploy Hugo site to Pages

   on:
     push:
       branches: ["main"]

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
           with:
             submodules: recursive
         - uses: peaceiris/actions-hugo@v2
           with:
             hugo-version: 'latest'
             extended: true
         - run: hugo --minify
         - uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_dir: ./public
   ```

## Option 3: Next.js with MDX

For a modern React-based blog with maximum flexibility.

### Setup

1. **Create Next.js app**
   ```bash
   npx create-next-app@latest my-blog
   cd my-blog
   ```

2. **Install MDX dependencies**
   ```bash
   npm install @next/mdx @mdx-js/loader @mdx-js/react
   npm install gray-matter
   ```

3. **Configure Next.js** (`next.config.js`):
   ```javascript
   const withMDX = require('@next/mdx')({
     extension: /\.mdx?$/,
   })

   module.exports = withMDX({
     pageExtensions: ['js', 'jsx', 'md', 'mdx'],
     images: {
       unoptimized: true,
     },
   })
   ```

4. **Copy content**
   ```bash
   mkdir -p content/posts
   cp ../blog-backup/content/custom-reporting-intune.md content/posts/

   mkdir -p public/images
   cp -r ../blog-backup/content/images/* public/images/
   ```

5. **Deploy to GitHub Pages**

   Add to `package.json`:
   ```json
   {
     "scripts": {
       "export": "next build && next export"
     }
   }
   ```

   Use GitHub Actions for automated deployment.

## Option 4: Gatsby

Blazing fast static site generator with React.

### Setup

1. **Install Gatsby CLI**
   ```bash
   npm install -g gatsby-cli
   ```

2. **Create a Gatsby site**
   ```bash
   gatsby new my-blog https://github.com/gatsbyjs/gatsby-starter-blog
   cd my-blog
   ```

3. **Copy content**
   ```bash
   cp ../blog-backup/content/custom-reporting-intune.md content/blog/2021-12-30-custom-reporting-intune/index.md
   cp -r ../blog-backup/content/images content/blog/2021-12-30-custom-reporting-intune/
   ```

4. **Deploy**
   ```bash
   npm run build

   # Deploy to GitHub Pages using gh-pages
   npm install gh-pages --save-dev
   npm run deploy
   ```

## Custom Domain Setup

Once your site is deployed, you can configure your custom domain:

1. **In GitHub repository settings:**
   - Go to Settings → Pages
   - Enter your custom domain: `justanothertech.blog`
   - Save

2. **Update DNS records with your domain registrar:**

   For apex domain (`justanothertech.blog`):
   ```
   A Record: 185.199.108.153
   A Record: 185.199.109.153
   A Record: 185.199.110.153
   A Record: 185.199.111.153
   ```

   For www subdomain (`www.justanothertech.blog`):
   ```
   CNAME: yourusername.github.io
   ```

3. **Enable HTTPS**
   - In GitHub Pages settings, check "Enforce HTTPS"

## Testing Locally

Before deploying, test your site locally:

### Jekyll
```bash
bundle exec jekyll serve
# Visit http://localhost:4000
```

### Hugo
```bash
hugo server -D
# Visit http://localhost:1313
```

### Next.js
```bash
npm run dev
# Visit http://localhost:3000
```

### Gatsby
```bash
gatsby develop
# Visit http://localhost:8000
```

## Recommended: Start with Jekyll

For your use case, I recommend starting with **Jekyll + GitHub Pages** because:

1. ✅ Native GitHub Pages support (zero configuration)
2. ✅ Free hosting
3. ✅ Automatic HTTPS
4. ✅ Built-in deployment on git push
5. ✅ Large theme ecosystem
6. ✅ Markdown support out of the box
7. ✅ Perfect for technical blogs

Your content is already in the perfect format for Jekyll with frontmatter and proper markdown structure.

## Need Help?

- Jekyll documentation: https://jekyllrb.com/docs/
- Hugo documentation: https://gohugo.io/documentation/
- Next.js documentation: https://nextjs.org/docs
- Gatsby documentation: https://www.gatsbyjs.com/docs/
- GitHub Pages docs: https://docs.github.com/en/pages
