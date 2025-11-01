# Blog Migration TODO

**Last Updated:** November 1, 2025
**Project:** Migrate justanothertech.blog from Wix to GitHub Pages

## ✅ Completed Tasks

- [x] Check existing backup contents
- [x] Navigate to 'All Posts' page to catalog all blog posts
- [x] Extract blog post content and convert to markdown
- [x] Download all images from the post (6 images, ~79 KB total)
- [x] Organize files in GitHub-ready structure
- [x] Create .gitignore file
- [x] Update README.md with complete backup documentation
- [x] Create DEPLOYMENT-GUIDE.md with deployment options
- [x] Add GitHub MCP server to Claude Code

## 🔄 In Progress

- [ ] Authenticate to GitHub MCP server (OAuth flow needed)

## 📋 Next Steps

### GitHub Setup
- [ ] Complete GitHub MCP authentication
- [ ] Create new GitHub repository (suggested name: `justanothertech-blog` or `blog`)
- [ ] Initialize git repository locally
- [ ] Push backup content to GitHub

### Choose Deployment Method
- [ ] Decide on static site generator:
  - Option A: Jekyll + GitHub Pages (recommended - zero config)
  - Option B: Hugo (faster builds)
  - Option C: Next.js (modern React-based)
  - Option D: Gatsby (React with GraphQL)

### Jekyll Setup (Recommended Path)
- [ ] Create `_config.yml` configuration file
- [ ] Choose and configure Jekyll theme (suggestions: Minima, Minimal Mistakes, Chirpy)
- [ ] Reorganize content for Jekyll:
  - [ ] Move blog post to `_posts/2021-12-30-custom-reporting-intune.md`
  - [ ] Move images to `assets/images/`
  - [ ] Update image paths in markdown from `./images/` to `/assets/images/`
- [ ] Test site locally with `bundle exec jekyll serve`
- [ ] Commit and push changes

### GitHub Pages Configuration
- [ ] Enable GitHub Pages in repository settings
- [ ] Set source branch to `main`
- [ ] Wait for initial deployment
- [ ] Test deployed site at `https://username.github.io/repo-name`

### Custom Domain Setup (Optional)
- [ ] Configure custom domain in GitHub Pages settings
- [ ] Update DNS records at domain registrar:
  - [ ] Add A records for apex domain
  - [ ] Add CNAME record for www subdomain
- [ ] Enable HTTPS enforcement
- [ ] Verify SSL certificate

### Final Testing
- [ ] Verify all images load correctly
- [ ] Check all internal links work
- [ ] Test external links to GitHub repo
- [ ] Validate responsive design on mobile
- [ ] Test site performance

### Cleanup
- [ ] Update old Wix site with redirect notice (before renewal expires Dec 30, 2025)
- [ ] Archive local backup after successful migration
- [ ] Document any issues encountered

## 📝 Notes

### Current Backup Location
`C:\Users\jackd\Downloads\blog-backup\`

### File Structure
```
blog-backup/
├── .gitignore
├── README.md
├── DEPLOYMENT-GUIDE.md
├── TODO.md (this file)
└── content/
    ├── custom-reporting-intune.md
    └── images/ (6 files)
```

### Important Dates
- **Wix Renewal:** December 30, 2025
- **Backup Created:** November 1, 2025
- **Target Go-Live:** Before December 30, 2025

### Resources
- [GitHub Repository (EnhancedLogging)](https://github.com/JackDDavis/EnhancedLogging)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- Deployment guide included in `DEPLOYMENT-GUIDE.md`

### GitHub Authentication Issue
- Added GitHub MCP server but connection failed (needs OAuth)
- Need to complete authentication flow with browser
- Command attempted: `claude mcp auth github` (command doesn't exist)
- Need to investigate proper OAuth flow for GitHub Copilot MCP endpoint

## 🤔 Questions to Resolve

1. What's the proper authentication method for GitHub Copilot MCP server?
2. Should we use the standard GitHub MCP server instead?
3. Repository name preference? (`blog`, `tech-blog`, `justanothertech-blog`)
4. Public or private repository?
5. Keep current domain or use GitHub Pages subdomain?

## 💡 Recommendations

1. **Start with Jekyll** - It's the path of least resistance for GitHub Pages
2. **Public repository** - Good for portfolio and open source contributions
3. **Use custom domain** - Better for professional branding
4. **Minimal Mistakes theme** - Great for technical blogs, well-documented

## 🆘 If Issues Arise

- All blog content is safely backed up at `C:\Users\jackd\Downloads\blog-backup\`
- Original site remains live at justanothertech.blog until Dec 30, 2025
- Markdown file includes all content with proper formatting
- All images downloaded locally
- Can pivot to any deployment method using DEPLOYMENT-GUIDE.md
