- [ ] Check there out first
    - [ ] Adobe
        - [ ] https://developer.adobe.com/document-services/docs/overview/pdf-embed-api/
        - [ ] https://acrobatservices.adobe.com/dc-integration-creation-app-cdn/main.html
        - [ ] DOCUMENTATION ANNOTATIOn - https://developer.adobe.com/document-services/docs/overview/pdf-embed-api/howtos_comments/#single-line-implementation
    - [ ] https://www.reddit.com/r/sveltejs/comments/1hte3q9/looking_for_pdf_viewer_that_works_with_svelte_5/
- [ ] Bugs on highlight page
    - [ ] use selectedText.split("\n") to determine if we should go nextSibling or previousSibling
    - [ ] a bug (sometimes) after highlighting, and click shift (maybe?) it will just fill the whole page with text
    - [ ] when replacing text, the text that are not encapsulated in <span> cannot be selected. Probably need to manipulate the replacement
        - [ ] it has something to do with how the `left` property is used
    - [ ] need to be able to highlight over multiple pages
    - [ ] when highlighting over multiple pages, it sometimes would highlight other things like page number, etc. Can't modify the pdf, so probably need to have a work around software side
    - [ ] when scrolling to other pages, the highlight can dissapear. Maybe have a detection of what page the user is in and rerender the highlight for that page