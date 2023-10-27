## blog with github action

### 
Blog posts from the main repository are automatically archived in the sub-repository via GitHub action.

### main repo github action workflow
```yaml
jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Ruby
        uses: ruby/setup-ruby@55283cc23133118229fd3f97f9336ee23a179fcf
        with:
          ruby-version: '3.1'
          bundler-cache: true
          cache-version: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
      - name: Pushes to another repository
        id: push_directory
        uses: cpina/github-action-push-to-another-repository@main
        env:
          API_TOKEN_GITHUB: ${{ secrets.MINGO_TOKEN }}
        with:
          source-directory: '_posts'
          destination-github-username: 'mingovvv'
          destination-repository-name: 'blog-with-github-action'
          commit-message: ${{ github.event.commits[0].message }}
          target-branch: main
          target-directory: '_posts'
```

### using library
> https://cpina.github.io/push-to-another-repository-docs/index.html
