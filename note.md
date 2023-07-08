## For website

1. bundle exec jekyll serve
2. all page font:html {
   line-height: 1.4;
   letter-spacing: 0.002em;
   font-size: 1rem;
   }
3. post card font setting:
   ```
   .post-card-meta {
   font-size: 0.8em;
   white-space: nowrap;
   display: block;
   padding-top: 4px;

   ```
4. post page font and margin, including data:
   ```
   .page-content {
   font-size: 1.02rem;
   margin: auto;
   margin-top: -80px;
   background-color: var(--background);
   padding: 20px;
   -webkit-border-top-left-radius: 20px;
   border-top-left-radius: 20px;
   -webkit-border-top-right-radius: 20px;
   border-top-right-radius: 20px;
   -webkit-box-shadow: 0px 2px 4px var(--shadow);
   box-shadow: 0px 2px 4px var(--shadow);
   /* page section width */
   /* max-width: 900px; */
   max-width: 90%;
   }
   ```

## For markdown

1. Replace | with \mid, replace + operator with\+, so as \*.
2. Replace `\{ \}` with `\\{ \\}` in inline equation.
