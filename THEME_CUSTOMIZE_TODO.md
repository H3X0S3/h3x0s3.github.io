# Fluid theme — customize for H3X0S3

The theme was copied from the reference repo (shengshengyang.github.io).
Edit `themes/fluid/_config.yml` and change these fields:

- `favicon`: replace with your own favicon path under source/assets/images/
- `navbar.blog_title`: "H3X0S3"
- `index.slogan.text`: your homepage subtitle
- `about.name` / `about.intro` / `about.icons`: your name, bio, social links
- `post.comments.utterances.repo`: change to `H3X0S3/h3x0s3.github.io`
  (and create the utterances config on that repo)
- `footer.content`: remove the Buy me a coffee widget or replace with yours
- `web_analytics.google.measurement_id`: set your GA4 id

Then run:
    npm install
    npx hexo server      # preview at http://localhost:4000
    npx hexo generate    # build into public/
