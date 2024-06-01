---
layout: post
title:  "[React] React-query pagination"
comments: true
date:  2023-03-15 15:07:33 +0700
categories: React
---


# React-query pagination

Github api with Pagination:

```
const url = `https://api.github.com/repos/${org}/${repo}/issues?page=${page}&per_page=${perPage}`;
```

Component will be

Since we pass `page` to the `useEffect` dependency array, the effect will run any time our page changes, automatically fetching the next page for us.

```
function fetchIssues({queryKey}) {
  const [issues, org, repo, {page, perPage}] = queryKey;
  const url = `https://api.github.com/repos/${org}/${repo}/issues?page=${page}&per_page=${perPage}`;
  return fetch(url).then(res => res.json());
}
```

```
const Issues = ({org, repo}) => {
  const [page, setPage] = useState(1);
  const perPage = 10;
  const issuesQuery = useQuery(
    ["issues", org, repo, {page, perPage}],
    fetchIssues,
    {keepPreviousData: true}
  );

  useEffect(() => {
    queryClient.prefetchQuery(
      ["issues", org, repo, {page: page + 1, perPage}],
      fetchIssues
    );
  }, [org, repo, page, perPage, queryClient]);

  return (
    <div>
      <div>
        {issuesQuery.loading && <div>Loading...</div>}
        {issuesQuery.error && <div>Error!</div>}
        {issuesQuery.data && issuesQuery.data.map(issue => (
          <Issue key={issue.id} issue={issue} />
        ))}
      </div>
      <div>
        <button
          onClick={() => setPage(page => page - 1)}
          disabled={page === 1}
        >
          Previous
        </button>
        <p>
          Page {page} {issuesQuery.isFetching ? "..." : ""}
        </p>
        <button
          onClick={() => setPage(page => page + 1)}
          disabled={
            !issuesQuery.data || issuesQuery.data.length === 0
          }
        >
          Next
        </button>
      </div>
    </div>
  );
}
```
