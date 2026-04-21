# How I made this site
I wanted to avoid having to update my actual site every time I wanted to change an image, word, or even have the ability to post articles.
I did not want to pay for or set up a cms like wordpress so I figured I could use github!
## APIS
You can simply use the urls to endpoints like this to get the manifest of your repo and the actual file contents.
```ts
const REPO = "shanehoughton/site-cms";
export const GITHUB_RAW_BASE = `https://raw.githubusercontent.com/${REPO}/main`;
export const GITHUB_CONTENTS_URL = `https://api.github.com/repos/${REPO}/contents`;
```
and then use simple get requests to fetch and display the content on the page somewhere:
```ts
import { GITHUB_RAW_BASE } from "../constants";

const PAGES_RAW_URL = `${GITHUB_RAW_BASE}/pages`;
const IMAGES_URL = `${GITHUB_RAW_BASE}/images`;

export async function getPageContent(pageName: string): Promise<string | null> {
  const res = await fetch(`${PAGES_RAW_URL}/${pageName}.md`);
  if (!res.ok) return null;

  const raw = await res.text();

  return raw;
}

export function getImageDownloadUrl(imageName: string): string {
  if (imageName.includes("https")) return imageName;
  return `${IMAGES_URL}/${imageName}`;
}
```

I used libraries like `react-markdown` and `remark-gfm` to render the md content from my cms repo, but just because I like markdown. But you could also render sanitized html files in a similar way.

The only thing I didnt bother doing was make my cms private, which you could probably do by setting up some serverless or server-side function, but it seemed overkill for a simple site.

You can find my top secret cms [here](https://github.com/ShaneHoughton/site-cms)