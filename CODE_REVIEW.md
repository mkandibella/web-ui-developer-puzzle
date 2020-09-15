# Code changes needed :

    1. More modularity needed in book searching code and reading list component. modularity helps in clean and structured code, Book-search.component.html can be broken in to two, Search input form and search response so that if we need similar functionality else where we can see the possible re-usability.


    2. There is memory leak issue in angular app.This will impact on performance as application size increase.Every Subscribe method must be unsubscribe to the observables on component destroy .**

    for example there is no need of 


    we can use selector to the observable async pipe in the HTML wgich will take care of the unsubscribing.
     
        
        The best way to unsubscribe method on ngOnDestroy lifecycle hook.
        for ex. 
        ```
        componentActive = true;
        
        ngOnInit(): void {
            this.store.select(getAllBooks).pipe(takeWhile(() => this.componentActive)).subscribe(books => {
            this.books = books;
            });
            }
        ngOnDestroy(): void {
            this.componentActive = false;
            }
        ```
    3. No need to inject service into app/feature module separately. Service should be inject itself in service like below.so its gives single instance of service in whole app.* 

        ```
        @Injectable({
        providedIn: 'root'
        })
        ```
    4. use async & trackByFn for *ngFor="let b of readingList$ | async;trackBy: trackByFn">

    trackBy
        When using ngFor to loop over an array in templates, use it with a trackBy function which will return an unique identifier for each item.

        Why?

        When an array changes, Angular re-renders the whole DOM tree. But if you use trackBy, Angular will know which element has changed and will only make DOM changes for that particular element.

    this is applied in reading-list.component.html and reading-list.component.ts
  
    5. Make books as observable *ngFor="let b of books | async"> AsyncPipes unsubscribe themselves automatically and it makes the code simpler by eliminating the need to manually manage subscriptions. It also reduces the risk of accidentally forgetting to unsubscribe a subscription in the component, which would cause a memory leak. This risk can also be mitigated by using a lint rule to detect unsubscribed observables.

    6. Follow a standard naming style (example: confirmedAction, ActionSuccess etc)
    
    7. Reading list should be user specific, 

       issue : Add a book to the reading list, open a new incognito window and check the reading list, reading list is populated with cart entries added in the normal browser session and this should be corrected.

    8. Should use Observable instead of Promise.Observable, since observables are more flexible than Promise**
    9. Date pipe does not work for null values. Should we consider using custom pipes here ?.
    10. formatDate can be handled in the html using pipes and this method can be removed.
    11. Page refresh is removing the search string, retaining search string in query parameter, this will help to maintain the state of the user on page refresh. 
    12. Fixed responsive issues.
    13. Few test cases are failing and Test cases need to be written for regarding list View appearing
  

# Accessibility

## Lighthouse :

    1. Button Label missing across the site.
    2. Background contrast color issue for  Reading list button
    3. Background contrast color issue for Empty search description (Try searching for a topic, for example)

## Manual :

    1. Image should have Alt attribute in order for the screen reader to read.
    2. Use the tabindex HTML attribute to explicitly set an element's tab position this provides logical navigation path through an application if you are forced to use sequential navigation
    3. Missing aria attribute :
        a. Missing aria-label ,  aria-live,  role  for the search results. (impact: Screen reader users may not be able to know that search results are loaded on the screen.)
        b. Missing aria-label  and aria-atomic for the search input field.
    4. Missing role attribute : 
        a. Missing second level of heading Role for book-title under book-items. (Impact: Screen reader users will not understand the relationship of the content under book-items because they may not know/understand which tile cells are headers).
        b. Missing Role for the Search form.
        c. Missing role for the book-items in book-search.component.html

Note: most of the  Manual and Lighthouse identified accessibility issues are fixed.