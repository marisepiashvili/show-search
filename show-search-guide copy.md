# სერიალების საძიებო: მენტორული გზამკვლევი

ეს ფაილი ნაბიჯ-ნაბიჯ გატარებს პროექტის აწყობას: **რა** ფაილი იქმნება, **სად**, **როგორ** იწერება და **რატომ** ასე. ეს არის გზამკვლევის TVmaze ვერსია და სრულად ანაცვლებს `movie-search-guide.md`-ს: პროექტი იგივეა, ოღონდ ფილმების ნაცვლად სერიალებს ეძებს და API-ს რეგისტრაცია არ სჭირდება.

კოდი აწყობილი და შემოწმებულია ამ ვერსიებზე (2026 წლის ოქტომბერი): Node 22.22, Vite 8.3, React 19.2, React Router 8.4, Tailwind CSS 4.3. ყოველი ეტაპის ბოლოს პროექტი იბილდება და `npm run lint` სუფთაა. ერთი შეზღუდვა: ჩემი სატესტო ბრაუზერი TVmaze-ის სერვერს ვერ წვდება. ამიტომ ნამდვილი პასუხების ფორმა ცალკე შევამოწმე (ძებნა, სია, დეტალები მსახიობებით, 404), აპი კი ბრაუზერში იმავე ფორმის იმიტირებულ პასუხებზე გავტესტე. თუ ნამდვილი პასუხი სადმე სხვანაირად მოიქცა, მომწერე.

> **VS Code-ში წასაკითხად:** გახსენი ეს ფაილი და დააჭირე `Ctrl+K`, შემდეგ `V` (Mac-ზე `Cmd+K`, `V`). გვერდით გაიხსნება ფორმატირებული ხედი. ფაილი პროექტის საქაღალდის გარეთ შეინახე, ან `.gitignore`-ში ჩაამატე, რომ GitHub-ზე არ აიტვირთოს.

## როგორ ვიმუშაოთ

1. **კოდი ხელით აკრიფე.** კოპირებისას ხელი არაფერს იმახსოვრებს. აკრეფისას შეცდომებს დაუშვებ, და სწორედ მათი გასწორებისას სწავლობ.
2. **ჯერ „რატომ" წაიკითხე, მერე დაწერე.** გასაუბრებაზე კოდს არ გკითხავენ, გკითხავენ რატომ დაწერე ასე.
3. **ყოველი ნაბიჯის შემდეგ შეამოწმე ბრაუზერში.** თუ რამე გაფუჭდა, იცი, რომ ბოლო ცვლილებამ გააფუჭა.
4. **ყოველი ეტაპის ბოლოს:** `npm run lint`, შემდეგ commit.
5. **თუ გაიჭედე:** ჯერ წაიკითხე შეცდომის ტექსტი ბრაუზერის Console-ში, შემდეგ შეადარე შენი ფაილი აქ მოცემულს. თუ 20 წუთში ვერ იპოვე, მომწერე შეცდომის ტექსტი და ფაილი.

## სარჩევი

| ეტაპი | რას აკეთებ | დრო |
|---|---|---|
| 0 | ინსტრუმენტები და API-ს გაცნობა | 20 წთ |
| 1 | პროექტის შექმნა და გამართვა | 1 სთ |
| 2 | API ფენა | 1 სთ |
| 3 | Routing და საერთო ჩარჩო | 1.5 სთ |
| 4 | მთავარი გვერდი: ტოპ სერიალები | 3 სთ |
| 5 | ძებნა | 3 სთ |
| 6 | დეტალური გვერდი | 3 სთ |
| 7 | რჩეულები | 2.5 სთ |
| 8 | გაპრიალება | 2 სთ |
| 9 | გაშვება და README | 1.5 სთ |

სულ დაახლოებით 19 საათი, ანუ 5-6 დღე დღეში 3-4 საათით. გეგმაში 4 დღე ეწერა, მაგრამ იქ ახსნების კითხვა და ხელით აკრეფა გათვლილი არ იყო. ნუ იჩქარებ: მიზანი გაგებაა და არა სისწრაფე.

საბოლოო სტრუქტურა, რომელსაც მიიღებ:

```
src/
├── main.jsx                  ← აპლიკაციის შესასვლელი
├── App.jsx                   ← ჩარჩო და route-ები
├── index.css
├── api/
│   └── tvmaze.js               ← სერვერთან საუბარი
├── utils/
│   └── format.js             ← წელი, რეიტინგი, ხანგრძლივობა, HTML-ის გასუფთავება
├── hooks/
│   ├── useDebounce.js
│   └── useFavorites.js
├── context/
│   ├── FavoritesContext.js
│   └── FavoritesProvider.jsx
├── components/               ← მრავალჯერადი გამოყენების ნაწილები
│   ├── Header.jsx, Footer.jsx, ScrollToTop.jsx
│   ├── Button.jsx, StatusMessage.jsx
│   ├── SearchBar.jsx, FavoriteButton.jsx
│   └── ShowCard.jsx, ShowGrid.jsx, ShowGridSkeleton.jsx, ShowResults.jsx
└── pages/                    ← თითო route-ს თითო გვერდი
    ├── HomePage.jsx, ShowDetailsPage.jsx
    └── FavoritesPage.jsx, NotFoundPage.jsx
```

**რატომ ასეთი დაყოფა:** თითო საქაღალდეს ერთი პასუხისმგებლობა აქვს. `api` სერვერს ელაპარაკება, `components` ხატავს, `hooks` ლოგიკას ინახავს, `pages` გვერდებს აწყობს. ახალი დეველოპერი წამებში პოულობს საჭირო ფაილს.

---

## ეტაპი 0. ინსტრუმენტები და API-ს გაცნობა

### 0.1. შეამოწმე Node და Git

```bash
node -v
git --version
```

Node უნდა იყოს **22.22 ან უფრო ახალი** (ამას React Router 8 მოითხოვს). თუ ძველია, nodejs.org-დან LTS ვერსია დააყენე.

### 0.2. VS Code-ის გაფართოებები

დააყენე სამი:

- **Prettier - Code formatter**: კოდს ავტომატურად აფორმატებს
- **Tailwind CSS IntelliSense**: კლასების ავტოშევსება და მინიშნებები
- **Oxc**: linter-ის შეცდომებს პირდაპირ რედაქტორში აჩვენებს

შემდეგ Settings-ში (`Ctrl+,`) ჩართე **Format On Save** და **Default Formatter**-ად აირჩიე Prettier.

**რატომ:** გუნდში ყველას კოდი ერთნაირად უნდა გამოიყურებოდეს. ფორმატირებაზე ფიქრი და კამათი დროის კარგვაა, ამას ინსტრუმენტი აკეთებს.

### 0.3. გაეცანი API-ს

გახსენი ბრაუზერში:

```
https://api.tvmaze.com/search/shows?q=batman
```

ეს არის ზუსტად ის, რასაც შენი აპი მიიღებს: JSON ტექსტი სერიალების სიით. Chrome-ში ზემოთ მონიშნე „Pretty-print", რომ წასაკითხი გახდეს.

TVmaze-ს რეგისტრაცია და გასაღები არ სჭირდება, ამიტომ კოდში დასამალი არაფერი გვაქვს. პირობა ერთია: საიტზე წყარო უნდა მიუთითო (ამას მე-3 ეტაპზე footer-ში გავაკეთებთ).

---

## ეტაპი 1. პროექტის შექმნა და გამართვა

**მიზანი:** ცარიელი, გამართული პროექტი, რომელშიც Tailwind მუშაობს.

### 1.1. შექმენი პროექტი

```bash
npm create vite@latest show-search -- --template react
cd show-search
npm install
npm run dev
```

თუ `npm create` ბოლოს გკითხავს, დააყენოს და გაუშვას თუ არა ახლავე, უპასუხე No: ხელით გავაკეთებთ, რომ ნახო რა ხდება.

გახსენი `http://localhost:5173`. უნდა ჩანდეს Vite-ის საწყისი გვერდი.

**რატომ Vite:** სწრაფი dev სერვერია და დღეს React პროექტის დასაწყებად სტანდარტული არჩევანია. Create React App მოძველებულია, ძველ ტუტორიალებში თუ შეგხვდა, გამოტოვე.

### 1.2. დააყენე ბიბლიოთეკები

```bash
npm install react-router tailwindcss @tailwindcss/vite
npm install -D prettier prettier-plugin-tailwindcss
```

**რატომ `-D`:** Prettier მხოლოდ დეველოპმენტისას გჭირდება, მომხმარებლის ბრაუზერში არ მიდის. ასეთი პაკეტები `devDependencies`-ში იწერება.

**ყურადღება:** პაკეტს ჰქვია `react-router`. `react-router-dom` ძველი სახელია, ნუ დააყენებ.

### 1.3. ჩართე Tailwind

**ფაილი:** `vite.config.js` (პროექტის ძირში, მთლიანად ჩაანაცვლე)

```js
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";
import { defineConfig } from "vite";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
});
```

**ფაილი:** `src/index.css` (წაშალე ყველაფერი, დატოვე ერთი ხაზი)

```css
@import "tailwindcss";
```

**რატომ:** Tailwind 4-ში კონფიგურაციის ფაილი აღარ არის საჭირო. Vite-ის plugin თვითონ პოულობს, რომელ კლასებს იყენებ, და მხოლოდ მათ ამატებს CSS-ში.

### 1.4. Prettier-ის კონფიგურაცია

**ფაილი:** `.prettierrc` (პროექტის ძირში, ახალი ფაილი)

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

**რატომ ეს plugin:** Tailwind-ის კლასებს ყოველთვის ერთი და იმავე თანმიმდევრობით ალაგებს. ეს Tailwind-ის ოფიციალური plugin-ია.

### 1.5. გაასუფთავე დემო კოდი

- წაშალე `src/App.css`
- წაშალე `src/assets` საქაღალდე
- წაშალე `public/icons.svg`

**ფაილი:** `index.html` (პროექტის ძირში, მთლიანად ჩაანაცვლე)

```html
<!doctype html>
<html lang="ka" class="scheme-dark">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta
      name="description"
      content="სერიალების საძიებო: მოძებნე სერიალი, ნახე დეტალები და შეინახე რჩეულებში."
    />
    <title>Show Search</title>
  </head>
  <body class="bg-zinc-950 text-zinc-100 antialiased">
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

რა შეიცვალა და რატომ:

- `lang="ka"`: ეკრანის წამკითხველმა და საძიებო სისტემამ უნდა იცოდეს, რა ენაზეა გვერდი. თუ ინტერფეისს ინგლისურად დაწერ, აქ `en` ჩაწერე.
- `class="scheme-dark"`: ბრაუზერს ეუბნება, რომ საიტი მუქია, და scrollbar-სა და ფორმის ელემენტებსაც მუქად ხატავს.
- `<meta name="description">`: ეს ტექსტი ჩანს Google-ის შედეგებში და ლინკის გაზიარებისას.
- `body`-ის კლასები: ფონისა და ტექსტის ფერი ერთ ადგილას, მთელი საიტისთვის.

**ფაილი:** `src/App.jsx` (მთლიანად ჩაანაცვლე)

```jsx
export default function App() {
  return <h1 className="p-8 text-3xl font-bold text-amber-400">Show Search</h1>;
}
```

**შეამოწმე:** ბრაუზერში უნდა ჩანდეს დიდი ყვითელი სათაური მუქ ფონზე. თუ ასეა, Tailwind მუშაობს.

ბოლოს ერთხელ დააფორმატე მთელი პროექტი, რომ Vite-ის შაბლონის ფაილებიც შენს სტილზე გადავიდეს:

```bash
npx prettier --write .
```

`src/main.jsx`-ს ჯერ არ ეხები. მასში არის `<StrictMode>`, არ წაშალო: ის development-ში შეცდომებს იჭერს, მე-2 ეტაპზე ნახავ როგორ.

### 1.6. შექმენი საქაღალდეები

`src`-ში შექმენი: `api`, `components`, `context`, `hooks`, `pages`, `utils`.

### 1.7. პირველი commit

GitHub-ზე შექმენი ცარიელი რეპოზიტორია `show-search` (README-ს და .gitignore-ის გარეშე).

```bash
git init
git add .
git status
```

**ყოველთვის წაიკითხე ეს სია commit-მდე.** `node_modules` მასში არ უნდა იყოს (`.gitignore` მას უკვე გამორიცხავს). ეს ჩვევა ერთ დღეს გადაგარჩენს პაროლის ან გასაღების შემთხვევით ატვირთვას.

```bash
git commit -m "chore: project setup with vite, tailwind and react router"
git branch -M main
git remote add origin https://github.com/შენი_სახელი/show-search.git
git push -u origin main
```

**რატომ ასეთი შეტყობინება:** ეს Conventional Commits ფორმატია: `feat` (ახალი ფუნქცია), `fix` (შეცდომის გასწორება), `chore` (გამართვა), `style` (ვიზუალი), `docs` (დოკუმენტაცია). ბევრი გუნდი იყენებს, და შენი commit-ების ისტორია რეკრუტერისთვის წასაკითხი იქნება.

---

## ეტაპი 2. API ფენა

**მიზანი:** ერთი ფაილი, რომელმაც იცის, როგორ ელაპარაკოს TVmaze-ს. კომპონენტებში `fetch` არსად ეწერება.

**ფაილი:** `src/api/tvmaze.js` (ახალი)

```js
const BASE_URL = "https://api.tvmaze.com";
const TOP_SHOWS_COUNT = 20;

async function request(path, { params = {}, signal } = {}) {
  const url = new URL(BASE_URL + path);
  for (const [key, value] of Object.entries(params)) {
    url.searchParams.set(key, value);
  }

  let response;
  try {
    response = await fetch(url, { signal });
  } catch (error) {
    if (error.name === "AbortError") throw error;
    throw new Error("სერვერთან დაკავშირება ვერ მოხერხდა. შეამოწმე ინტერნეტი.", {
      cause: error,
    });
  }

  if (!response.ok) {
    const message =
      response.status === 429
        ? "ძალიან ბევრი მოთხოვნაა. სცადე რამდენიმე წამში."
        : `მოთხოვნა ვერ შესრულდა (კოდი ${response.status}).`;
    const error = new Error(message);
    error.status = response.status;
    throw error;
  }

  return response.json();
}

export async function getTopShows(signal) {
  const shows = await request("/shows", { params: { page: 0 }, signal });

  return shows
    .filter((show) => show.rating.average !== null)
    .sort((a, b) => b.rating.average - a.rating.average)
    .slice(0, TOP_SHOWS_COUNT);
}

export async function searchShows(query, signal) {
  const results = await request("/search/shows", {
    params: { q: query },
    signal,
  });

  return results.map((result) => result.show);
}

export function getShowDetails(id, signal) {
  return request(`/shows/${id}`, { params: { embed: "cast" }, signal });
}
```

გავარჩიოთ ნაწილ-ნაწილ.

**რატომ ცალკე ფაილი.** კომპონენტმა იცის მხოლოდ `getTopShows()`, და არ იცის არც URL, არც პასუხის ფორმა. ამის სარგებელი ამ პროექტზევე ჩანს: თავიდან ის სხვა API-ზე (TMDB) იყო დაგეგმილი. TVmaze-ზე გადასვლისას ეს ფაილი თავიდან დაიწერა, კომპონენტებში კი მხოლოდ ველების სახელები შეიცვალა.

**`request` ფუნქცია** არ არის `export`. ის ამ ფაილის შიდა დეტალია, გარეთ მხოლოდ სამი გასაგები სახელის ფუნქცია გადის.

**`new URL(...)` და `searchParams.set`.** URL-ს ხელით ნუ აწებებ (`"?q=" + text`). მომხმარებელმა შეიძლება აკრიფოს `tom & jerry` ან ქართული ტექსტი, და `&` მისამართს გააფუჭებს. `searchParams.set` ამ სიმბოლოებს თვითონ აკოდირებს.

**`try / catch` `fetch`-ის გარშემო.** `fetch` შეცდომას აგდებს მხოლოდ მაშინ, როცა სერვერამდე ვერ მიაღწია (ინტერნეტი გათიშულია). ბრაუზერის ტექსტი ამ დროს არის „Failed to fetch", რაც მომხმარებელს არაფერს ეუბნება, ამიტომ გასაგები ტექსტით ვანაცვლებთ. `cause`-ში თავდაპირველი შეცდომა რჩება დებაგისთვის.

**`AbortError`-ს უცვლელად ვაგდებთ.** ეს შეცდომა არ არის, ეს ნიშნავს, რომ მოთხოვნა ჩვენ თვითონ გავაუქმეთ. მე-4 ეტაპზე ნახავ რატომ.

**`if (!response.ok)`.** დამწყებების ყველაზე გავრცელებული შეცდომა: `fetch` 404-ზე და 500-ზე შეცდომას **არ** აგდებს. თუ `response.ok`-ს არ შეამოწმებ, შეცდომის JSON-ს სერიალების სიად მიიღებ. `error.status`-ში კოდს ვინახავთ, რომ მე-6 ეტაპზე 404 სხვა შეცდომებისგან გავარჩიოთ.

**კოდი 429.** TVmaze ერთ IP-ს 10 წამში მინიმუმ 20 მოთხოვნის უფლებას აძლევს. ლიმიტის გადაჭარბებისას აბრუნებს 429-ს („Too Many Requests"), და ამ შემთხვევისთვის ცალკე, გასაგები ტექსტი გვაქვს.

**`signal` პარამეტრი.** მისი საშუალებით გამომძახებელს შეუძლია მოთხოვნის გაუქმება.

**მონაცემების მოწესრიგება API ფენაში.** ეს ამ ფაილის მთავარი იდეაა. TVmaze-ის ორი endpoint სხვადასხვა ფორმის პასუხს აბრუნებს:

- `/shows?page=0` აბრუნებს სერიალების მასივს: `[{ id, name, ... }, ...]`
- `/search/shows` აბრუნებს მასივს, სადაც სერიალი ერთი დონით ღრმადაა: `[{ score, show: { id, name, ... } }, ...]`

`searchShows` ამას `.map((result) => result.show)`-ით ასწორებს. შედეგად ორივე ფუნქცია ერთსა და იმავეს აბრუნებს: სერიალების მასივს. კომპონენტს აღარ სჭირდება იმის ცოდნა, საიდან მოვიდა სია.

**`getTopShows`.** TVmaze-ს „პოპულარულების" endpoint არ აქვს. ამიტომ ვიღებთ კატალოგის პირველ გვერდს (250 სერიალი) და თვითონ ვარჩევთ 20 ყველაზე მაღალრეიტინგულს:

- `.filter(...)` ტოვებს მხოლოდ მათ, ვისაც რეიტინგი აქვს (ზოგს `null` აქვს)
- `.sort((a, b) => b.rating.average - a.rating.average)` ალაგებს კლებადობით
- `.slice(0, TOP_SHOWS_COUNT)` იღებს პირველ 20-ს

`sort` მასივს ადგილზე ცვლის, მაგრამ აქ ეს უსაფრთხოა, რადგან `filter`-მა უკვე ახალი მასივი შექმნა. `TOP_SHOWS_COUNT` ფაილის თავშია, რომ რიცხვი 20 კოდში უსახელოდ არ ეგდოს.

> **რა გადავწყვიტეთ და რა ფასად:** 250 სერიალის ჩამოტვირთვა 20-ის საჩვენებლად ზედმეტია. TVmaze-ს უფრო პატარა ვარიანტი არ აქვს. ნამდვილ პროდუქტში ამ დახარისხებას backend გააკეთებდა და ბრაუზერს მხოლოდ 20-ს გაუგზავნიდა. სასწავლო პროექტისთვის ეს მისაღებია, მაგრამ გასაუბრებაზე თუ გკითხეს, ზუსტად ასე უპასუხე.

### შეამოწმე

**ფაილი:** `src/App.jsx` (დროებით, მთლიანად ჩაანაცვლე)

```jsx
import { useEffect } from "react";
import { getTopShows } from "./api/tvmaze";

export default function App() {
  useEffect(() => {
    getTopShows().then((data) => console.log(data));
  }, []);

  return <h1 className="p-8 text-3xl font-bold text-amber-400">Show Search</h1>;
}
```

გახსენი DevTools (`F12`) → Console. უნდა ნახო მასივი 20 სერიალით. გახსენი ერთი და დააკვირდი ველებს: `id`, `name`, `premiered`, `genres`, `rating.average`, `image.medium`, `summary`. ამ ველებს გამოიყენებ შემდეგ ეტაპებზე.

**მასივი ორჯერ დაიბეჭდა?** ასეც უნდა იყოს. `StrictMode` development-ში ყოველ effect-ს ორჯერ უშვებს, რომ გაჩვენოს, სწორად ასუფთავებ თუ არა მას. production-ში ერთხელ გაეშვება.

თუ შეცდომაა:

| რას ხედავ | მიზეზი |
|---|---|
| `სერვერთან დაკავშირება ვერ მოხერხდა` | ინტერნეტი, VPN ან რეკლამის ბლოკერი |
| `კოდი 404` | მისამართში შეცდომაა. შეადარე `BASE_URL` და გზები აქ მოცემულს |
| `ძალიან ბევრი მოთხოვნაა` | ლიმიტს გადააჭარბე, დაელოდე 10 წამი |

DevTools → Network ჩანართშიც შეიხედე: დააჭირე `shows?page=0` მოთხოვნას და ნახე Response.

**Commit:** `feat: add tvmaze api client`

---

## ეტაპი 3. Routing და საერთო ჩარჩო

**მიზანი:** ოთხი მისამართი ოთხ გვერდს ხსნის, ყველა გვერდზე ერთი header და footer ჩანს.

### 3.1. ჩართე router

**ფაილი:** `src/main.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router";
import App from "./App.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
);
```

**რატომ:** `BrowserRouter` უსმენს მისამართის ცვლილებას და ქვემოთ მყოფ კომპონენტებს ატყობინებს. ყველაფერი, რაც routing-ს იყენებს (`Link`, `useParams`), მის შიგნით უნდა იყოს, ამიტომ მთელ აპს ვახვევთ.

### 3.2. გვერდები

ჯერ მხოლოდ სათაურით, რომ routing შევამოწმოთ. შიგთავსს შემდეგ ეტაპებზე დავწერთ.

**ფაილი:** `src/pages/HomePage.jsx`

```jsx
export default function HomePage() {
  return <h1 className="text-2xl font-bold">მთავარი გვერდი</h1>;
}
```

**ფაილი:** `src/pages/FavoritesPage.jsx`

```jsx
export default function FavoritesPage() {
  return <h1 className="text-2xl font-bold">რჩეულები</h1>;
}
```

**ფაილი:** `src/pages/ShowDetailsPage.jsx`

```jsx
import { useParams } from "react-router";

export default function ShowDetailsPage() {
  const { id } = useParams();

  return <h1 className="text-2xl font-bold">სერიალის გვერდი, id: {id}</h1>;
}
```

`useParams()` მისამართიდან კითხულობს დინამიურ ნაწილს. `/show/169`-ზე `id` იქნება `"550"`. ყურადღება: ეს ყოველთვის **სტრიქონია**, რიცხვი არა.

**ფაილი:** `src/pages/NotFoundPage.jsx` (ეს უკვე საბოლოო ვერსიაა)

```jsx
import { Link } from "react-router";

export default function NotFoundPage() {
  return (
    <div className="py-16 text-center">
      <title>გვერდი ვერ მოიძებნა · Show Search</title>
      <h1 className="text-5xl font-bold">404</h1>
      <p className="mt-4 text-zinc-400">ასეთი გვერდი არ არსებობს.</p>
      <Link
        to="/"
        className="mt-6 inline-block text-amber-400 underline underline-offset-4"
      >
        მთავარზე დაბრუნება
      </Link>
    </div>
  );
}
```

**`<title>` კომპონენტის შიგნით?** დიახ. React 19 ამ თეგს თვითონ გადაიტანს `<head>`-ში, და გვერდიდან გასვლისას წაშლის. ასე თითო გვერდს თავისი სათაური აქვს ბრაუზერის ჩანართში, `useEffect`-ისა და `document.title`-ის გარეშე.

### 3.3. Header და Footer

**ფაილი:** `src/components/Header.jsx`

```jsx
import { Link, NavLink } from "react-router";

function getNavLinkClass({ isActive }) {
  return isActive ? "text-amber-400" : "text-zinc-300 hover:text-white";
}

export default function Header() {
  return (
    <header className="sticky top-0 z-10 border-b border-zinc-800 bg-zinc-950/90 backdrop-blur">
      <div className="mx-auto flex max-w-6xl items-center justify-between px-4 py-4">
        <Link to="/" className="text-xl font-bold tracking-tight">
          Show<span className="text-amber-400">Search</span>
        </Link>

        <nav
          aria-label="მთავარი ნავიგაცია"
          className="flex gap-6 text-sm font-medium"
        >
          <NavLink to="/" end className={getNavLinkClass}>
            მთავარი
          </NavLink>
          <NavLink to="/favorites" className={getNavLinkClass}>
            რჩეულები
          </NavLink>
        </nav>
      </div>
    </header>
  );
}
```

- **`Link` და არა `<a>`:** `<a href>` მთელ გვერდს თავიდან ტვირთავს და React-ის state იკარგება. `Link` მხოლოდ მისამართს ცვლის და საჭირო კომპონენტს ხატავს.
- **`NavLink`:** იგივეა, ოღონდ იცის, აქტიურია თუ არა. `className`-ში ფუნქციას ვაწვდით, რომელიც `isActive`-ს იღებს.
- **`end`:** ამის გარეშე `/` აქტიური იქნებოდა ყველა გვერდზე, რადგან ყველა მისამართი `/`-ით იწყება.
- **`getNavLinkClass` კომპონენტის გარეთ:** ის კომპონენტის state-ს არ იყენებს, ამიტომ ყოველ render-ზე თავიდან შექმნა არ სჭირდება.
- **`<header>`, `<nav>`, `aria-label`:** სემანტიკური თეგებით ეკრანის წამკითხველი ხვდება, სად არის ნავიგაცია. `div`-ებით ამას ვერ გაიგებს.
- **`sticky top-0`:** header გადახვევისას ზემოთ რჩება.

**ფაილი:** `src/components/Footer.jsx`

```jsx
export default function Footer() {
  return (
    <footer className="border-t border-zinc-800">
      <p className="mx-auto max-w-6xl px-4 py-6 text-center text-sm text-zinc-400">
        მონაცემების წყარო:{" "}
        <a
          href="https://www.tvmaze.com"
          target="_blank"
          rel="noreferrer"
          className="text-amber-400 underline underline-offset-4"
        >
          TVmaze
        </a>{" "}
        (CC BY-SA)
      </p>
    </footer>
  );
}
```

- **რატომ ეს ტექსტი:** TVmaze-ის მონაცემები CC BY-SA ლიცენზიითაა: გამოყენება უფასოა, მაგრამ წყარო უნდა მიუთითო.
- **აქ `<a>` და არა `Link`:** `Link` მხოლოდ შენი აპის შიდა გვერდებისთვისაა. სხვა საიტზე ჩვეულებრივი `<a href>` გადადის.
- **`target="_blank"` და `rel="noreferrer"`:** ლინკი ახალ ჩანართში იხსნება, და გახსნილ საიტს არ გადაეცემა ინფორმაცია იმაზე, საიდან მოვიდა მომხმარებელი.
- **`{" "}`:** JSX ხაზის ბოლოში space-ს შლის. თუ სიტყვასა და თეგს შორის space გჭირდება, ასე ჩაწერე.

### 3.4. App: ჩარჩო და route-ები

**ფაილი:** `src/App.jsx` (მთლიანად ჩაანაცვლე, დროებითი `console.log` ქრება)

```jsx
import { Route, Routes } from "react-router";
import Footer from "./components/Footer";
import Header from "./components/Header";
import FavoritesPage from "./pages/FavoritesPage";
import HomePage from "./pages/HomePage";
import ShowDetailsPage from "./pages/ShowDetailsPage";
import NotFoundPage from "./pages/NotFoundPage";

export default function App() {
  return (
    <div className="flex min-h-dvh flex-col">
      <Header />

      <main className="mx-auto w-full max-w-6xl flex-1 px-4 py-8">
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/show/:id" element={<ShowDetailsPage />} />
          <Route path="/favorites" element={<FavoritesPage />} />
          <Route path="*" element={<NotFoundPage />} />
        </Routes>
      </main>

      <Footer />
    </div>
  );
}
```

- **`Routes` / `Route`:** მისამართს ადარებს `path`-ს და ხატავს შესაბამის `element`-ს. `Header` და `Footer` `Routes`-ის გარეთაა, ამიტომ ყველა გვერდზე ჩანს.
- **`/show/:id`:** ორწერტილი ნიშნავს ცვლადს. სწორედ ამას კითხულობს `useParams`.
- **`path="*"`:** ყველაფერი დანარჩენი. ამის გარეშე არასწორ მისამართზე ცარიელი გვერდი გამოჩნდებოდა.
- **`flex min-h-dvh flex-col` და `flex-1`:** footer ყოველთვის ეკრანის ბოლოშია, მაშინაც კი, როცა გვერდზე ცოტა შიგთავსია. `dvh` მობილურზე ბრაუზერის მისამართის ზოლს ითვალისწინებს, `vh` არა.
- **`mx-auto max-w-6xl px-4`:** შიგთავსი ცენტრშია, ფართო ეკრანზე არ იწელება, მობილურზე კიდეებს არ ეკვრის. იგივე სიგანე აქვს header-სა და footer-ს, ამიტომ ყველაფერი ერთ ხაზზეა.

### შეამოწმე

- `/` → „მთავარი გვერდი", ნავიგაციაში „მთავარი" ყვითელია
- `/favorites` → „რჩეულები", ნავიგაციაში ის არის ყვითელი
- `/show/123` → „სერიალის გვერდი, id: 123"
- `/რამე` → 404, ჩანართის სათაური შეიცვალა
- ლინკებზე დაჭერისას გვერდი **არ** ციმციმებს (თავიდან არ იტვირთება)

**Commit:** `feat: add routing and layout`

---

## ეტაპი 4. მთავარი გვერდი: ტოპ სერიალები

**მიზანი:** მთავარ გვერდზე ჩანს 20 სერიალის ბადე, ჩატვირთვისას skeleton, შეცდომისას შეტყობინება ღილაკით.

წესი: **ჯერ პატარა ნაწილები, მერე დიდი.** ქვემოდან ზემოთ ვაწყობთ.

### 4.1. ფორმატირების ფუნქციები

**ფაილი:** `src/utils/format.js` (ახალი)

```js
export function getYear(dateString) {
  return dateString ? dateString.slice(0, 4) : "—";
}

export function formatRating(value) {
  return value ? value.toFixed(1) : "—";
}

export function formatRuntime(minutes) {
  return minutes ? `${minutes} წთ` : "—";
}
```

**რატომ ცალკე:** ეს ფუნქციები React-ს არ ეხება. იღებენ მნიშვნელობას, აბრუნებენ ტექსტს. ასეთს „სუფთა ფუნქცია" ჰქვია, ადვილია მისი გამოყენება რამდენიმე ადგილას და ტესტირება.

**რატომ ამდენი შემოწმება:** API-ს მონაცემს არასდროს ენდო. TVmaze-ის ძებნის შედეგებში `rating.average` ხშირად `null`-ია (ახალ ან ნაკლებად ცნობილ სერიალებზე), `premiered` და `averageRuntime`-იც შეიძლება არ იყოს. შემოწმების გარეშე აპი `null.slice`-ზე ან `null.toFixed`-ზე გაჩერდებოდა და მომხმარებელი თეთრ ეკრანს ნახავდა.

### 4.2. ღილაკი და შეტყობინება

**ფაილი:** `src/components/Button.jsx` (ახალი)

```jsx
export default function Button({ children, ...props }) {
  return (
    <button
      type="button"
      className="rounded-lg bg-amber-400 px-4 py-2 font-medium text-zinc-950 transition hover:bg-amber-300"
      {...props}
    >
      {children}
    </button>
  );
}
```

- **`type="button"`:** ფორმის შიგნით `<button>` ნაგულისხმევად `submit`-ია და ფორმას აგზავნის. ყოველთვის მიუთითე ტიპი.
- **`...props`:** ყველაფერი, რასაც გადასცემ (`onClick`, `disabled`), პირდაპირ `<button>`-ზე გადადის. ასე კომპონენტი ჩვეულებრივი ღილაკივით იქცევა.

**ფაილი:** `src/components/StatusMessage.jsx` (ახალი)

```jsx
export default function StatusMessage({ title, description, children }) {
  return (
    <div role="status" className="py-16 text-center">
      <p className="text-lg font-medium">{title}</p>
      {description && <p className="mt-2 text-zinc-400">{description}</p>}
      {children && <div className="mt-6">{children}</div>}
    </div>
  );
}
```

**`children`:** ის, რასაც კომპონენტის გამხსნელ და დამხურავ თეგებს შორის ჩაწერ. ასე `StatusMessage`-მა არ იცის, ღილაკი ექნება შიგნით, ლინკი თუ არაფერი. ამას გამომძახებელი წყვეტს. `role="status"` ეკრანის წამკითხველს ატყობინებს, რომ ეს ტექსტი ხმამაღლა წასაკითხია.

### 4.3. Skeleton

**ფაილი:** `src/components/ShowGridSkeleton.jsx` (ახალი)

```jsx
export default function ShowGridSkeleton({ count = 10 }) {
  return (
    <div role="status">
      <span className="sr-only">იტვირთება…</span>

      <div
        aria-hidden="true"
        className="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5"
      >
        {Array.from({ length: count }, (_, index) => (
          <div key={index} className="animate-pulse">
            <div className="aspect-2/3 rounded-lg bg-zinc-800" />
            <div className="mt-2 h-4 w-3/4 rounded bg-zinc-800" />
            <div className="mt-2 h-3 w-1/2 rounded bg-zinc-800" />
          </div>
        ))}
      </div>
    </div>
  );
}
```

- **რატომ skeleton და არა spinner:** skeleton იმავე ადგილს იკავებს, რასაც მერე ნამდვილი ბარათები. გვერდი ჩატვირთვისას არ „ხტება", და მომხმარებელს ელოდება ნაკლებად ეჩვენება.
- **`Array.from({ length: count }, ...)`:** ქმნის `count` ელემენტიან მასივს.
- **`key={index}` აქ დასაშვებია.** ეს სია არასდროს იცვლება და ელემენტებს საკუთარი id არ აქვთ. ნამდვილ მონაცემებზე index-ს ნუ გამოიყენებ (იხ. 4.5).
- **`sr-only` და `aria-hidden`:** ნაცრისფერი ბლოკები ეკრანის წამკითხველს არაფერს ეუბნება, ამიტომ მათ ვმალავთ მისგან, და სანაცვლოდ ვაძლევთ უხილავ ტექსტს „იტვირთება…".

### 4.4. სერიალის ბარათი

**ფაილი:** `src/components/ShowCard.jsx` (ახალი)

```jsx
import { Link } from "react-router";
import { formatRating, getYear } from "../utils/format";

export default function ShowCard({ show }) {
  const posterUrl = show.image?.medium;

  return (
    <article className="group relative">
      <Link to={`/show/${show.id}`} className="block rounded-lg">
        <div className="aspect-2/3 overflow-hidden rounded-lg bg-zinc-800">
          {posterUrl ? (
            <img
              src={posterUrl}
              alt=""
              loading="lazy"
              className="size-full object-cover transition duration-300 group-hover:scale-105"
            />
          ) : (
            <div className="flex size-full items-center justify-center p-4 text-center text-sm text-zinc-400">
              პოსტერი არ არის
            </div>
          )}
        </div>

        <h2 className="mt-2 line-clamp-1 font-medium">{show.name}</h2>
        <p className="text-sm text-zinc-400">
          {getYear(show.premiered)} · ★ {formatRating(show.rating.average)}
        </p>
      </Link>
    </article>
  );
}
```

- **`aspect-2/3` კონტეინერი:** ადგილს იკავებს მანამ, სანამ სურათი ჩაიტვირთება. ამის გარეშე ბარათები სურათის მოსვლისას ხტება. ამას Layout Shift ჰქვია და Lighthouse ქულას აკლებს.
- **`alt=""`:** შეცდომა არ არის. პოსტერის ქვემოთ სათაური უკვე წერია, და `alt`-ში იგივეს გამეორებით ეკრანის წამკითხველი ორჯერ წაიკითხავდა. დეკორატიულ სურათს ცარიელი `alt` უნდა ჰქონდეს. მაგრამ ატრიბუტი აუცილებლად უნდა ეწეროს: მის გარეშე წამკითხველი ფაილის სახელს კითხულობს.
- **`loading="lazy"`:** სურათი იტვირთება მხოლოდ მაშინ, როცა ეკრანს უახლოვდება.
- **`show.image?.medium`:** TVmaze სურათს ობიექტად აბრუნებს ორი ზომით (`medium` და `original`), ან `null`-ს, თუ სურათი არ აქვს. `?.` (optional chaining) ნიშნავს: თუ `image` `null`-ია, შეცდომით ნუ გაჩერდები, დააბრუნე `undefined`. ბარათზე `medium` საკმარისია, დიდი სურათის ჩამოტვირთვა ტრაფიკის ფლანგვაა.
- **`posterUrl ? ... : ...`:** უპოსტერო სერიალზე გატეხილი სურათის ნაცვლად ტექსტი ჩანს.
- **`group` და `group-hover:`:** მშობელზე მაუსის მიტანისას შვილი იცვლება. აქ მთელ ბარათზე მიტანისას სურათი ოდნავ იზრდება.
- **`line-clamp-1`:** გრძელი სათაური ერთ ხაზზე იჭრება სამი წერტილით, და ბარათები ერთი სიმაღლის რჩება.
- **`<article>` და `<h2>`:** გვერდის მთავარი სათაური `h1`-ია, ბარათების სათაურები მის ქვეშ `h2`.

### 4.5. ბადე

**ფაილი:** `src/components/ShowGrid.jsx` (ახალი)

```jsx
import ShowCard from "./ShowCard";

export default function ShowGrid({ shows }) {
  return (
    <ul className="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5">
      {shows.map((show) => (
        <li key={show.id}>
          <ShowCard show={show} />
        </li>
      ))}
    </ul>
  );
}
```

- **`key={show.id}`:** React-ს სჭირდება უნიკალური, სტაბილური იდენტიფიკატორი, რომ გაიგოს, რომელი ელემენტი დაემატა, წაიშალა ან გადაადგილდა. index-ის გამოყენებისას სიის შეცვლა state-ს ურევს.
- **`<ul>` / `<li>`:** ეს სიაა, და ეკრანის წამკითხველი იტყვის „სია, 20 ელემენტი".
- **`grid-cols-2 sm:... md:... lg:...`:** Tailwind mobile-first-ია. პრეფიქსის გარეშე კლასი მობილურისთვისაა, `sm:` 640px-დან, `md:` 768px-დან, `lg:` 1024px-დან.

### 4.6. მონაცემების ჩატვირთვა

ეს ამ ეტაპის მთავარი ფაილია. ყურადღებით წაიკითხე.

**ფაილი:** `src/components/ShowResults.jsx` (ახალი)

```jsx
import { useEffect, useState } from "react";
import { getTopShows } from "../api/tvmaze";
import Button from "./Button";
import ShowGrid from "./ShowGrid";
import ShowGridSkeleton from "./ShowGridSkeleton";
import StatusMessage from "./StatusMessage";

export default function ShowResults() {
  const [status, setStatus] = useState("loading");
  const [shows, setShows] = useState([]);
  const [errorMessage, setErrorMessage] = useState("");
  const [attempt, setAttempt] = useState(0);

  useEffect(() => {
    const controller = new AbortController();

    async function loadShows() {
      try {
        const data = await getTopShows(controller.signal);

        setShows(data);
        setStatus("success");
      } catch (error) {
        if (error.name === "AbortError") return;

        setErrorMessage(error.message);
        setStatus("error");
      }
    }

    loadShows();

    return () => controller.abort();
  }, [attempt]);

  function handleRetry() {
    setStatus("loading");
    setAttempt((current) => current + 1);
  }

  if (status === "loading") {
    return <ShowGridSkeleton />;
  }

  if (status === "error") {
    return (
      <StatusMessage
        title="სერიალების ჩატვირთვა ვერ მოხერხდა"
        description={errorMessage}
      >
        <Button onClick={handleRetry}>სცადე თავიდან</Button>
      </StatusMessage>
    );
  }

  return <ShowGrid shows={shows} />;
}
```

**`status` ერთი სტრიქონით.** ხშირად ნახავ `isLoading` და `isError` ორ ცალკე boolean-ს. მაშინ შესაძლებელი ხდება უაზრო კომბინაცია: ორივე `true`. ერთი `status` სამი მნიშვნელობით (`"loading"`, `"success"`, `"error"`) ამას გამორიცხავს.

**საწყისი მნიშვნელობა `"loading"`.** კომპონენტი გამოჩენისთანავე იწყებს ჩატვირთვას, ამიტომ პირველივე render-ზე skeleton ჩანს.

**`async` ფუნქცია effect-ის შიგნით.** `useEffect(async () => ...)` არ შეიძლება: effect-მა უნდა დააბრუნოს გამწმენდი ფუნქცია ან არაფერი, `async` კი ყოველთვის Promise-ს აბრუნებს. ამიტომ შიგნით ვქმნით ფუნქციას და იქვე ვიძახებთ.

**`AbortController` და `return () => controller.abort()`.** effect-ის მიერ დაბრუნებული ფუნქცია ეშვება, როცა კომპონენტი ქრება ან effect თავიდან ეშვება. ჩვენ იქ მოთხოვნას ვაუქმებთ. რატომ არის ეს საჭირო:

- მომხმარებელი სხვა გვერდზე გადავიდა. პასუხი აღარავის სჭირდება.
- შემდეგ ეტაპზე ძებნისას ძველი პასუხი ახალზე გვიან რომ არ მოვიდეს და არასწორი შედეგი არ აჩვენოს.

**`if (error.name === "AbortError") return;`** გაუქმება შეცდომა არ არის. ამ ხაზის გარეშე მომხმარებელი ნახავდა შეცდომის შეტყობინებას, როცა უბრალოდ სხვა გვერდზე გადავიდა.

**`attempt`.** „სცადე თავიდან" ღილაკი უბრალოდ ზრდის რიცხვს. რადგან `attempt` dependency მასივშია, effect თავიდან ეშვება. `setAttempt((current) => current + 1)` ფუნქციური ფორმაა: ახალი მნიშვნელობა წინაზე დაყრდნობით.

**dependency მასივი `[attempt]`.** აქ ჩამოთვლი ყველა ცვლადს, რომელსაც effect იყენებს და რომელიც render-ებს შორის შეიძლება შეიცვალოს. მასივის გამოტოვებისას effect ყოველ render-ზე გაეშვება და უსასრულო მოთხოვნებს მიიღებ.

**early return-ები ბოლოში.** ჯერ გამონაკლისები (`loading`, `error`), ბოლოს ჩვეულებრივი შემთხვევა. ერთმანეთში ჩადგმულ `? :`-ებზე ბევრად ადვილად იკითხება.

> **შენიშვნა ნამდვილ პროექტებზე:** გუნდებში მონაცემების ჩატვირთვას ხშირად ბიბლიოთეკა აკეთებს (TanStack Query), რომელიც ამ ყველაფერს და ქეშირებასაც თვითონ აგვარებს. მაგრამ ჯერ ხელით უნდა დაწერო, რომ გესმოდეს, რას აკეთებს ის შენ მაგივრად. გასაუბრებაზე სწორედ ამას გკითხავენ.

### 4.7. მთავარი გვერდი

**ფაილი:** `src/pages/HomePage.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import ShowResults from "../components/ShowResults";

export default function HomePage() {
  return (
    <>
      <h1 className="mb-4 text-2xl font-bold">ტოპ სერიალები</h1>

      <ShowResults />
    </>
  );
}
```

`<>...</>` არის Fragment: რამდენიმე ელემენტს აერთიანებს ზედმეტი `div`-ის გარეშე.

### შეამოწმე

- მთავარ გვერდზე ჩანს 20 ბარათი, მობილურის სიგანეზე 2 სვეტად, ფართო ეკრანზე 5-ად (DevTools-ში `Ctrl+Shift+M`)
- **Skeleton:** DevTools → Network → throttling → „Slow 4G", განაახლე გვერდი
- **შეცდომა:** Network → „Offline", განაახლე. უნდა ჩანდეს შეტყობინება და ღილაკი. დააბრუნე „No throttling" და დააჭირე „სცადე თავიდან"
- **StrictMode:** Network-ში შეიძლება ნახო ორი `shows?page=0` მოთხოვნა, რომელთაგან პირველი „canceled"-ია. ეს შენი `abort()`-ია მოქმედებაში და მხოლოდ development-ში ხდება
- Console-ში შეცდომა და გაფრთხილება არ არის

**Commit:** `feat: show top shows on home page`

---

## ეტაპი 5. ძებნა

**მიზანი:** მომხმარებელი წერს, შედეგები ავტომატურად იცვლება, ძებნის ტექსტი URL-შია.

ეს პროექტის მთავარი ნაწილია. გასაუბრებაზე ყველაზე მეტს ამაზე ისაუბრებ.

### 5.1. Debounce hook

**პრობლემა:** „batman"-ის აკრეფა 6 ასოა. ყოველ ასოზე მოთხოვნის გაგზავნა 6 მოთხოვნაა, რომელთაგან 5 არავის სჭირდება.

**გადაწყვეტა:** დაველოდოთ, სანამ მომხმარებელი წერას შეწყვეტს.

**ფაილი:** `src/hooks/useDebounce.js` (ახალი)

```js
import { useEffect, useState } from "react";

export function useDebounce(value, delay = 400) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timeoutId = setTimeout(() => setDebouncedValue(value), delay);

    return () => clearTimeout(timeoutId);
  }, [value, delay]);

  return debouncedValue;
}
```

როგორ მუშაობს: ყოველ ჯერზე, როცა `value` იცვლება, effect ჯერ **ასუფთავებს წინა ტაიმერს** (`clearTimeout`) და ახალს რთავს. ამიტომ ტაიმერი ბოლომდე მხოლოდ მაშინ მიდის, როცა 400 მილიწამის განმავლობაში ახალი ასო არ აკრეფილა.

**რა არის custom hook:** ჩვეულებრივი ფუნქცია, რომლის სახელი `use`-ით იწყება და რომელიც სხვა hook-ებს იძახებს. ლოგიკას კომპონენტიდან გამოყოფ და ნებისმიერ ადგილას იყენებ. მაღაზიის პროექტშიც გამოგადგება.

### 5.2. ძებნის ველი

**ფაილი:** `src/components/SearchBar.jsx` (ახალი)

```jsx
export default function SearchBar({ value, onChange }) {
  return (
    <form role="search" onSubmit={(event) => event.preventDefault()}>
      <label htmlFor="show-search" className="sr-only">
        სერიალის ძებნა
      </label>
      <input
        id="show-search"
        type="search"
        value={value}
        onChange={(event) => onChange(event.target.value)}
        placeholder="მოძებნე სერიალი…"
        autoComplete="off"
        className="w-full rounded-lg border border-zinc-700 bg-zinc-900 px-4 py-3 placeholder:text-zinc-500"
      />
    </form>
  );
}
```

- **controlled input:** ველის მნიშვნელობა (`value`) გარედან მოდის და ყოველი ცვლილება `onChange`-ით გარეთ გადის. კომპონენტს საკუთარი state არ აქვს, ის „სულელია", და ეს კარგია: ადვილად გამოიყენებ სხვაგანაც.
- **`<label>` + `sr-only`:** ველს ყოველთვის უნდა ჰქონდეს label. `placeholder` მას ვერ ანაცვლებს, რადგან აკრეფისას ქრება და ეკრანის წამკითხველი ყოველთვის არ კითხულობს. `sr-only` label-ს ვიზუალურად მალავს, წამკითხველისთვის კი ტოვებს. `htmlFor` და `id` ერთმანეთს უნდა ემთხვეოდეს.
- **`type="search"`:** მობილურზე კლავიატურას ძებნის ღილაკი უჩნდება, ბრაუზერი კი ველს გასასუფთავებელ × ღილაკს უმატებს.
- **`<form role="search">` და `preventDefault`:** ეკრანის წამკითხველი ხვდება, რომ ეს საძიებო ზონაა. `preventDefault` იმიტომ, რომ Enter-ზე ფორმა ნაგულისხმევად გვერდს ტვირთავს თავიდან.
- **`autoComplete="off"`:** ბრაუზერის ძველი აკრეფილი ტექსტების სია შედეგებს რომ არ გადაეფაროს.

### 5.3. მთავარი გვერდი: ყველაფრის შეერთება

**ფაილი:** `src/pages/HomePage.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import { useSearchParams } from "react-router";
import ShowResults from "../components/ShowResults";
import SearchBar from "../components/SearchBar";
import { useDebounce } from "../hooks/useDebounce";

export default function HomePage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const inputValue = searchParams.get("q") ?? "";
  const query = useDebounce(inputValue.trim());

  function handleSearchChange(value) {
    setSearchParams(value ? { q: value } : {}, { replace: true });
  }

  return (
    <>
      <SearchBar value={inputValue} onChange={handleSearchChange} />

      <h1 className="mt-8 mb-4 text-2xl font-bold">
        {query ? `ძებნის შედეგები: „${query}“` : "ტოპ სერიალები"}
      </h1>

      <ShowResults key={query} query={query} />
    </>
  );
}
```

აქ ოთხი მნიშვნელოვანი იდეაა.

**1. ძებნის ტექსტი URL-ში ინახება და არა `useState`-ში.** `useSearchParams` მუშაობს როგორც `useState`, ოღონდ მნიშვნელობა მისამართშია (`/?q=batman`). რას იღებ ამით უფასოდ:

- გვერდის განახლებისას ძებნა არ იკარგება
- ლინკის გაზიარება შეიძლება
- სერიალის გვერდიდან „უკან" დაბრუნებისას შედეგები ადგილზეა
- ლოგოზე დაჭერა (`/`) ძებნას ასუფთავებს

პრინციპი: **ერთი სიმართლის წყარო.** ტექსტი `useState`-შიც რომ გქონდეს და URL-შიც, მათი სინქრონიზაცია მოგიწევდა, და სწორედ იქ ჩნდება შეცდომები.

**2. `replace: true`.** ამის გარეშე ყოველი ასო ისტორიაში ახალ ჩანაწერს დაამატებდა, და „batman"-ის შემდეგ „უკან" ღილაკს 6-ჯერ უნდა დააჭირო.

**3. ორი მნიშვნელობა: `inputValue` და `query`.** `inputValue` მაშინვე იცვლება, რომ ველში ასო დაუყოვნებლივ გამოჩნდეს. `query` მისი დაგვიანებული ვერსიაა, და მოთხოვნა მხოლოდ მას მიჰყვება. `.trim()` აშორებს კიდეებზე space-ებს, რომ „batman " და „batman" ორ სხვადასხვა ძებნად არ ჩაითვალოს.

**4. `key={query}`.** ეს React-ის ერთ-ერთი ყველაზე სასარგებლო ხერხია. როცა `key` იცვლება, React ძველ კომპონენტს მთლიანად აგდებს და ახალს ქმნის, **სუფთა state-ით**. ანუ ახალ ძებნაზე `status` თავისით ხდება `"loading"`, ძველი სერიალები და ძველი შეცდომა ქრება. ამის გარეშე effect-ში ხელით უნდა გეწერა `setStatus("loading")`. React-ის დოკუმენტაციაც state-ის გასასუფთავებლად `key`-ს ურჩევს.

`??` ოპერატორი: თუ მარცხენა მხარე `null` ან `undefined`-ია, იღებს მარჯვენას. `searchParams.get("q")` აბრუნებს `null`-ს, როცა პარამეტრი არ არის.

### 5.4. ShowResults: ძებნის მხარდაჭერა

**ფაილი:** `src/components/ShowResults.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import { useEffect, useState } from "react";
import { getTopShows, searchShows } from "../api/tvmaze";
import Button from "./Button";
import ShowGrid from "./ShowGrid";
import ShowGridSkeleton from "./ShowGridSkeleton";
import StatusMessage from "./StatusMessage";

export default function ShowResults({ query }) {
  const [status, setStatus] = useState("loading");
  const [shows, setShows] = useState([]);
  const [errorMessage, setErrorMessage] = useState("");
  const [attempt, setAttempt] = useState(0);

  useEffect(() => {
    const controller = new AbortController();

    async function loadShows() {
      try {
        const data = query
          ? await searchShows(query, controller.signal)
          : await getTopShows(controller.signal);

        setShows(data);
        setStatus("success");
      } catch (error) {
        if (error.name === "AbortError") return;

        setErrorMessage(error.message);
        setStatus("error");
      }
    }

    loadShows();

    return () => controller.abort();
  }, [query, attempt]);

  function handleRetry() {
    setStatus("loading");
    setAttempt((current) => current + 1);
  }

  if (status === "loading") {
    return <ShowGridSkeleton />;
  }

  if (status === "error") {
    return (
      <StatusMessage
        title="სერიალების ჩატვირთვა ვერ მოხერხდა"
        description={errorMessage}
      >
        <Button onClick={handleRetry}>სცადე თავიდან</Button>
      </StatusMessage>
    );
  }

  if (shows.length === 0) {
    return (
      <StatusMessage
        title="ვერაფერი მოიძებნა"
        description="სცადე სხვა სათაური ან შეამოწმე მართლწერა."
      />
    );
  }

  return <ShowGrid shows={shows} />;
}
```

რა შეიცვალა წინა ვერსიასთან შედარებით (5 ადგილი):

1. import-ში დაემატა `searchShows`
2. კომპონენტი იღებს `{ query }` prop-ს
3. `query` თუ არის, ვეძებთ, თუ არა, ტოპ სერიალებს ვტვირთავთ
4. dependency მასივი გახდა `[query, attempt]`, რადგან effect ახლა `query`-საც იყენებს
5. დაემატა ცარიელი შედეგის შემთხვევა

**ცარიელი შედეგი შეცდომა არ არის.** სერვერმა წარმატებით უპასუხა, უბრალოდ სია ცარიელია. ამას საკუთარი შეტყობინება სჭირდება, „სცადე თავიდან" ღილაკის გარეშე.

TVmaze ძებნაზე მაქსიმუმ 10 შედეგს აბრუნებს, ამიტომ ძებნის შედეგებში 20 ბარათს ნუ ელი.

### 5.5. ერთი ცვლილება router-ში

**ფაილი:** `src/main.jsx` (ერთი ხაზი იცვლება: `BrowserRouter`-ს ემატება prop)

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router";
import App from "./App.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter useTransitions={false}>
      <App />
    </BrowserRouter>
  </StrictMode>,
);
```

**რატომ `useTransitions={false}`.** ეს ნამდვილი პრობლემაა, რომელსაც პროექტის ტესტირებისას წააწყდები. React Router ნაგულისხმევად URL-ის ყოველ ცვლილებას „დაბალი პრიორიტეტის" განახლებად ნიშნავს (`startTransition`). ჩვეულებრივ ეს კარგია, მაგრამ ჩვენი ველის მნიშვნელობა URL-დან მოდის, controlled input-ს კი მნიშვნელობა **დაუყოვნებლივ** უნდა განუახლდეს. დაგვიანებისას ბრაუზერი კურსორს ტექსტის ბოლოში აგდებს.

თვითონ ნახე: დროებით წაშალე ეს prop, აკრიფე „batman", დააწკაპუნე სიტყვის შუაში და აკრიფე ორი ასო. მეორე ასო ბოლოში აღმოჩნდება. დააბრუნე prop და პრობლემა ქრება.

> **რა გადავწყვიტეთ და რა ფასად:** URL ყოველ ასოზე იწერება (ეს იაფია, მხოლოდ მისამართის ზოლი იცვლება), მოთხოვნა კი debounce-ით იგზავნება. ალტერნატივაა ველის ტექსტის ცალკე `useState`-ში შენახვა და URL-ის დაგვიანებით განახლება, მაგრამ მაშინ ორი state-ის ხელით სინქრონიზაცია გიწევს (ლოგოზე დაჭერა, „უკან" ღილაკი). ამ ზომის პროექტისთვის პირველი გზა უფრო მარტივი და საიმედოა. გასაუბრებაზე ასეთ არჩევანზე საუბარი ძალიან ფასდება.

### შეამოწმე

- აკრიფე „batman" სწრაფად. Network-ში უნდა იყოს **ერთი** `search/shows` მოთხოვნა (development-ში შეიძლება ორი ჩანდეს, პირველი „canceled", StrictMode-ის გამო)
- მისამართი გახდა `/?q=batman`, სათაური შეიცვალა
- განაახლე გვერდი (`F5`): ძებნა ადგილზეა
- დააჭირე ლოგოს: ველი გასუფთავდა, ტოპ სერიალები დაბრუნდა
- აკრიფე უაზრო ტექსტი, მაგალითად `qzxjvkwp`: „ვერაფერი მოიძებნა". TVmaze-ის ძებნა აკრეფის შეცდომებს პატიობს, ამიტომ ზოგ უაზრო ტექსტზე შეიძლება მაინც რამე იპოვოს. მაშინ სხვა სცადე
- დააწკაპუნე ტექსტის შუაში და აკრიფე: კურსორი ადგილზე რჩება
- **race condition:** ჩართე „Slow 4G", აკრიფე „bat", დაელოდე ნახევარი წამი, დაამატე „man". ბოლოს „batman"-ის შედეგები უნდა ჩანდეს და არა „bat"-ის. ეს `AbortController`-ის დამსახურებაა

**Commit:** `feat: add search with debounce and url sync`

---

## ეტაპი 6. დეტალური გვერდი

**მიზანი:** ბარათზე დაჭერით იხსნება სერიალის გვერდი პოსტერით, აღწერით, ჟანრებითა და მსახიობებით.

### 6.1. აღწერის გასუფთავება

TVmaze აღწერას HTML-ად აბრუნებს: `"<p><b>Breaking Bad</b> follows...</p>"`. JSX-ში პირდაპირ რომ ჩაწერო, მომხმარებელი თეგებს ტექსტად დაინახავს.

**ფაილი:** `src/utils/format.js` (ბოლოში დაემატა `stripHtml`)

```js
export function getYear(dateString) {
  return dateString ? dateString.slice(0, 4) : "—";
}

export function formatRating(value) {
  return value ? value.toFixed(1) : "—";
}

export function formatRuntime(minutes) {
  return minutes ? `${minutes} წთ` : "—";
}

export function stripHtml(html) {
  if (!html) return "";

  const parsed = new DOMParser().parseFromString(html, "text/html");

  return parsed.body.textContent.trim();
}
```

**რატომ არა `dangerouslySetInnerHTML`.** React-ს აქვს ეს prop, რომელიც HTML-ს პირდაპირ გვერდში სვამს. სახელი შემთხვევით არ ჰქვია ასე: თუ მონაცემებში ვინმემ ჩაწერა `<img src=x onerror="...">`, მისი კოდი შენს საიტზე, შენი მომხმარებლის ბრაუზერში გაეშვება. ამ შეტევას XSS ჰქვია. წესი: **სხვისგან მოსულ HTML-ს გვერდში არასდროს სვამ.**

**როგორ მუშაობს `stripHtml`.** `DOMParser` HTML-ს ცალკე, უხილავ დოკუმენტად შლის. ამ დოკუმენტში სკრიპტები არ ეშვება და სურათები არ იტვირთება. იქიდან ვიღებთ მხოლოდ `textContent`-ს, ანუ სუფთა ტექსტს თეგების გარეშე. ამავე დროს `&amp;` ხდება `&`.

`if (!html) return "";` იმიტომ, რომ ზოგ სერიალს აღწერა საერთოდ არ აქვს (`null`).

### 6.2. გვერდი

**ფაილი:** `src/pages/ShowDetailsPage.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import { useEffect, useState } from "react";
import { Link, useLocation, useNavigate, useParams } from "react-router";
import { getShowDetails } from "../api/tvmaze";
import Button from "../components/Button";
import StatusMessage from "../components/StatusMessage";
import {
  formatRating,
  formatRuntime,
  getYear,
  stripHtml,
} from "../utils/format";

const MAX_CAST = 8;

export default function ShowDetailsPage() {
  const { id } = useParams();
  const navigate = useNavigate();
  const location = useLocation();

  const [status, setStatus] = useState("loading");
  const [show, setShow] = useState(null);
  const [loadError, setLoadError] = useState(null);
  const [attempt, setAttempt] = useState(0);

  useEffect(() => {
    const controller = new AbortController();

    async function loadShow() {
      try {
        const data = await getShowDetails(id, controller.signal);

        setShow(data);
        setStatus("success");
      } catch (error) {
        if (error.name === "AbortError") return;

        setLoadError(error);
        setStatus("error");
      }
    }

    loadShow();

    return () => controller.abort();
  }, [id, attempt]);

  function handleRetry() {
    setStatus("loading");
    setAttempt((current) => current + 1);
  }

  function handleBack() {
    if (location.key === "default") {
      navigate("/");
    } else {
      navigate(-1);
    }
  }

  if (status === "loading") {
    return (
      <div role="status" className="animate-pulse">
        <span className="sr-only">იტვირთება…</span>
        <div className="grid gap-8 md:grid-cols-[300px_1fr]">
          <div className="mx-auto aspect-2/3 w-full max-w-75 rounded-lg bg-zinc-800" />
          <div className="space-y-4">
            <div className="h-9 w-2/3 rounded bg-zinc-800" />
            <div className="h-4 w-1/3 rounded bg-zinc-800" />
            <div className="h-24 rounded bg-zinc-800" />
          </div>
        </div>
      </div>
    );
  }

  if (status === "error") {
    if (loadError.status === 404) {
      return (
        <StatusMessage
          title="სერიალი ვერ მოიძებნა"
          description="შესაძლოა ბმული არასწორია."
        >
          <Link to="/" className="text-amber-400 underline underline-offset-4">
            მთავარზე დაბრუნება
          </Link>
        </StatusMessage>
      );
    }

    return (
      <StatusMessage
        title="სერიალის ჩატვირთვა ვერ მოხერხდა"
        description={loadError.message}
      >
        <Button onClick={handleRetry}>სცადე თავიდან</Button>
      </StatusMessage>
    );
  }

  const posterUrl = show.image?.original;
  const channel = show.network?.name ?? show.webChannel?.name;
  const summary = stripHtml(show.summary);
  const cast = show._embedded.cast.slice(0, MAX_CAST);

  return (
    <article>
      <title>{`${show.name} · Show Search`}</title>

      <button
        type="button"
        onClick={handleBack}
        className="mb-6 text-sm text-zinc-300 hover:text-white"
      >
        ← უკან
      </button>

      <div className="grid gap-8 md:grid-cols-[300px_1fr]">
        <div className="mx-auto aspect-2/3 w-full max-w-75 overflow-hidden rounded-lg bg-zinc-800">
          {posterUrl ? (
            <img
              src={posterUrl}
              alt={`${show.name} — პოსტერი`}
              className="size-full object-cover"
            />
          ) : (
            <div className="flex size-full items-center justify-center text-zinc-400">
              პოსტერი არ არის
            </div>
          )}
        </div>

        <div>
          <h1 className="text-3xl font-bold">{show.name}</h1>

          <p className="mt-4 text-sm text-zinc-300">
            {getYear(show.premiered)} · ეპიზოდი:{" "}
            {formatRuntime(show.averageRuntime)} · ★{" "}
            {formatRating(show.rating.average)}
          </p>

          {channel && (
            <p className="mt-1 text-sm text-zinc-400">არხი: {channel}</p>
          )}

          {show.genres.length > 0 && (
            <ul className="mt-4 flex flex-wrap gap-2">
              {show.genres.map((genre) => (
                <li
                  key={genre}
                  className="rounded-full bg-zinc-800 px-3 py-1 text-sm"
                >
                  {genre}
                </li>
              ))}
            </ul>
          )}

          <h2 className="mt-8 text-xl font-semibold">აღწერა</h2>
          <p className="mt-2 max-w-prose leading-relaxed text-zinc-300">
            {summary || "აღწერა არ არის."}
          </p>

          {cast.length > 0 && (
            <>
              <h2 className="mt-8 text-xl font-semibold">მსახიობები</h2>
              <ul className="mt-4 grid grid-cols-2 gap-4 sm:grid-cols-4">
                {cast.map(({ person, character }) => {
                  const photoUrl = person.image?.medium;

                  return (
                    <li key={`${person.id}-${character.id}`}>
                      <div className="aspect-2/3 overflow-hidden rounded-lg bg-zinc-800">
                        {photoUrl && (
                          <img
                            src={photoUrl}
                            alt=""
                            loading="lazy"
                            className="size-full object-cover"
                          />
                        )}
                      </div>
                      <p className="mt-2 text-sm font-medium">{person.name}</p>
                      <p className="text-sm text-zinc-400">{character.name}</p>
                    </li>
                  );
                })}
              </ul>
            </>
          )}
        </div>
      </div>
    </article>
  );
}
```

ჩატვირთვის ნაწილი იგივე ნიმუშია, რაც `ShowResults`-ში. ახალი აქ ეს არის:

**`MAX_CAST` ფაილის თავში.** კოდში „ჯადოსნურ რიცხვებს" ნუ ჩაწერ. `slice(0, 8)`-ის წამკითხველმა არ იცის, რა არის 8. `MAX_CAST` კი თვითონ ხსნის თავს.

**ერთი მოთხოვნა ორის ნაცვლად.** `tvmaze.js`-ში `getShowDetails` აგზავნის `embed=cast`-ს. ამით სერიალის მონაცემებიც და მსახიობებიც ერთ პასუხში მოდის, მსახიობები `_embedded.cast` ველში.

**`[id, attempt]`.** effect `id`-ს იყენებს, ამიტომ ის მასივში უნდა იყოს.

**`loadError` state-ში მთელი შეცდომის ობიექტია** და არა მხოლოდ ტექსტი, რადგან `status` გვჭირდება: 404-ზე („ასეთი სერიალი არ არსებობს") „სცადე თავიდან" ღილაკს აზრი არ აქვს, ამიტომ სხვა შეტყობინებას ვაჩვენებთ. `/show/999999999` სწორედ ამ შემთხვევაა.

**`handleBack`.** `navigate(-1)` ბრაუზერის „უკან" ღილაკივით მუშაობს და მომხმარებელს ძებნის შედეგებზე აბრუნებს. მაგრამ თუ მან სერიალის ლინკი პირდაპირ გახსნა (ვიღაცამ გაუგზავნა), „უკან" მას საიტიდან გაიყვანდა. `location.key === "default"` ნიშნავს, რომ ეს აპში პირველი გვერდია, და ამ შემთხვევაში მთავარზე გადაგვყავს.

**`<title>`.** ჩანართში სერიალის სახელი ჩანს. ტექსტი ერთ template string-ად იმიტომ არის, რომ `<title>`-ს შიგნით მხოლოდ ერთი ტექსტი უნდა იყოს.

**`show.image?.original`.** დეტალურ გვერდზე პოსტერი დიდია, ამიტომ აქ სურათის დიდ ვერსიას ვიღებთ.

**`show.network?.name ?? show.webChannel?.name`.** ტელევიზიით გასულ სერიალს `network` აქვს (მაგალითად AMC). სტრიმინგ-სერვისის სერიალს `network` `null` აქვს და სანაცვლოდ `webChannel` (მაგალითად Netflix). `?.` იცავს `null`-ისგან, `??` კი იღებს მარჯვენა მხარეს, თუ მარცხენა ცარიელია.

**`channel && ...` და `show.genres.length > 0 && ...`.** `&&` ხატავს მარჯვენა მხარეს, თუ მარცხენა ჭეშმარიტია. **ფრთხილად რიცხვებთან:** `{cast.length && <ul>}` ცარიელ მასივზე ეკრანზე დახატავს `0`-ს, რადგან `0` React-ისთვის ჩვეულებრივი ტექსტია. ამიტომ ყოველთვის წერე `length > 0`.

**`summary || "აღწერა არ არის."`** ცარიელი სტრიქონის შემთხვევაში იღებს მარჯვენა მხარეს.

**`key={genre}`.** TVmaze-ში ჟანრები უბრალო სტრიქონებია (`"Drama"`), id არ აქვთ. ერთ სერიალში ჟანრი არ მეორდება, ამიტომ თვითონ ტექსტი გამოდგება key-დ.

**`cast.map(({ person, character }) => ...)`.** მსახიობების სიის ყოველი ელემენტი არის `{ person, character }`: ვინ თამაშობს და ვის. ფრჩხილებში დაშლით (destructuring) ორივეს პირდაპირ ვიღებთ.

**`key={`${person.id}-${character.id}`}`.** ერთი მსახიობი ერთ სერიალში შეიძლება ორ როლს თამაშობდეს, და მაშინ `person.id` განმეორდება. მსახიობისა და როლის წყვილი კი უნიკალურია.

**დეტალურ გვერდზე პოსტერს `alt` აქვს,** რადგან აქ სურათი მთავარი შიგთავსია. მსახიობების ფოტოებს ისევ `alt=""`, რადგან სახელი ქვემოთ წერია.

**`md:grid-cols-[300px_1fr]`.** კვადრატულ ფრჩხილებში ნებისმიერი CSS მნიშვნელობა იწერება. აქ: 768px-დან ორი სვეტი, პირველი 300px, მეორე დანარჩენი. მობილურზე ერთი სვეტია.

### 6.3. გადახვევის პრობლემა

გადადი მთავარი გვერდის ბოლოში და დააჭირე ბოლო ბარათს. სერიალის გვერდი **შუიდან** გაიხსნება. რატომ: ჩვეულებრივ საიტზე ახალი გვერდი თავიდან იტვირთება და ბრაუზერი ზემოთ ადის. ჩვენთან გვერდი არ იტვირთება, მხოლოდ შიგთავსი იცვლება, ამიტომ გადახვევა ძველ ადგილზე რჩება.

**ფაილი:** `src/components/ScrollToTop.jsx` (ახალი)

```jsx
import { useEffect } from "react";
import { useLocation, useNavigationType } from "react-router";

export default function ScrollToTop() {
  const { pathname } = useLocation();
  const navigationType = useNavigationType();

  useEffect(() => {
    if (navigationType !== "POP") {
      window.scrollTo(0, 0);
    }
  }, [pathname, navigationType]);

  return null;
}
```

- **`return null`:** კომპონენტი არაფერს ხატავს, მხოლოდ effect-ს უშვებს. ეს ნორმალური ნიმუშია.
- **`navigationType !== "POP"`:** `POP` არის „უკან / წინ" ღილაკი. ამ დროს ზემოთ ასვლა არ გვინდა, ბრაუზერმა თვითონ უნდა დააბრუნოს ძველი პოზიცია.
- ეს effect-ის სწორი გამოყენებაა: React-ის state-ს (მისამართს) ვუსინქრონებთ გარე სისტემას (ბრაუზერის გადახვევას).

**ფაილი:** `src/App.jsx` (დაემატა ორი ხაზი: import და `<ScrollToTop />`)

```jsx
import { Route, Routes } from "react-router";
import Footer from "./components/Footer";
import Header from "./components/Header";
import ScrollToTop from "./components/ScrollToTop";
import FavoritesPage from "./pages/FavoritesPage";
import HomePage from "./pages/HomePage";
import ShowDetailsPage from "./pages/ShowDetailsPage";
import NotFoundPage from "./pages/NotFoundPage";

export default function App() {
  return (
    <div className="flex min-h-dvh flex-col">
      <ScrollToTop />
      <Header />

      <main className="mx-auto w-full max-w-6xl flex-1 px-4 py-8">
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/show/:id" element={<ShowDetailsPage />} />
          <Route path="/favorites" element={<FavoritesPage />} />
          <Route path="*" element={<NotFoundPage />} />
        </Routes>
      </main>

      <Footer />
    </div>
  );
}
```

### შეამოწმე

- ბარათზე დაჭერით იხსნება სერიალის გვერდი, ჩანართში სერიალის სახელია
- „← უკან" გაბრუნებს იმავე ძებნის შედეგებზე
- გახსენი `/show/169` ახალ ჩანართში პირდაპირ, დააჭირე „← უკან": მთავარზე გადადიხარ
- `/show/999999999` → „სერიალი ვერ მოიძებნა"
- სიის ბოლო ბარათზე დაჭერისას ახალი გვერდი ზემოდან იხსნება
- აღწერაში `<p>` და `<b>` თეგები არ ჩანს
- მობილურის სიგანეზე პოსტერი ზემოთაა, ტექსტი ქვემოთ, არაფერი სცდება ეკრანს

**Commit:** `feat: add show details page`

---

## ეტაპი 7. რჩეულები

**მიზანი:** გულზე დაჭერით სერიალი ინახება, ჩანს ცალკე გვერდზე და ბრაუზერის დახურვის შემდეგაც რჩება.

### 7.1. რატომ Context

რჩეულების სია სჭირდება: Header-ს (რაოდენობა), ყველა ბარათს (გული სავსეა თუ ცარიელი), დეტალურ გვერდს და რჩეულების გვერდს. state `App`-ში რომ შეგენახა, prop-ად უნდა ჩაგეტანა `App → HomePage → ShowResults → ShowGrid → ShowCard → FavoriteButton`. ამას prop drilling ჰქვია: შუალედური კომპონენტები იღებენ prop-ს, რომელიც თვითონ არ სჭირდებათ.

**Context** ამას აგვარებს: მონაცემს ერთ ადგილას „აცხადებ", და ნებისმიერი კომპონენტი პირდაპირ კითხულობს.

სამ ფაილად ვყოფთ. მიზეზი ქვემოთაა.

**ფაილი:** `src/context/FavoritesContext.js` (ახალი)

```js
import { createContext } from "react";

export const FavoritesContext = createContext(null);
```

**ფაილი:** `src/context/FavoritesProvider.jsx` (ახალი)

```jsx
import { useEffect, useState } from "react";
import { FavoritesContext } from "./FavoritesContext";

const STORAGE_KEY = "show-search:favorites";

function readFavorites() {
  try {
    const saved = JSON.parse(localStorage.getItem(STORAGE_KEY));
    return Array.isArray(saved) ? saved : [];
  } catch {
    return [];
  }
}

function pickCardFields(show) {
  return {
    id: show.id,
    name: show.name,
    image: show.image,
    premiered: show.premiered,
    rating: show.rating,
  };
}

export default function FavoritesProvider({ children }) {
  const [favorites, setFavorites] = useState(readFavorites);

  useEffect(() => {
    try {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(favorites));
    } catch {
      // localStorage სავსეა ან გამორთულია: აპი მაინც მუშაობს, უბრალოდ არ ინახავს
    }
  }, [favorites]);

  function isFavorite(id) {
    return favorites.some((show) => show.id === id);
  }

  function toggleFavorite(show) {
    setFavorites((current) =>
      current.some((item) => item.id === show.id)
        ? current.filter((item) => item.id !== show.id)
        : [...current, pickCardFields(show)],
    );
  }

  return (
    <FavoritesContext value={{ favorites, isFavorite, toggleFavorite }}>
      {children}
    </FavoritesContext>
  );
}
```

- **`STORAGE_KEY = "show-search:favorites"`:** `localStorage` ერთ დომენზე ყველა აპისთვის საერთოა. პრეფიქსით სხვის გასაღებს არ გადაეწერები.
- **`useState(readFavorites)`, ფრჩხილების გარეშე.** ფუნქციის გადაცემისას React მას მხოლოდ პირველ render-ზე იძახებს. `useState(readFavorites())` რომ დაგეწერა, `localStorage` ყოველ render-ზე წაიკითხებოდა ტყუილად.
- **`try / catch` წაკითხვისას:** `localStorage` შეიძლება გამორთული იყოს (ზოგი ბრაუზერის private რეჟიმი), ან შიგნით გაფუჭებული ტექსტი ეწეროს. `JSON.parse` ამ დროს შეცდომას აგდებს და მთელი აპი თეთრ ეკრანად იქცეოდა. `Array.isArray` კი ამოწმებს, რომ წაკითხული ნამდვილად მასივია.
- **`useEffect` ჩაწერისთვის:** ეს effect-ის სწორი გამოყენებაა, state-ს ვუსინქრონებთ გარე სისტემას. `[favorites]` ნიშნავს: ჩაწერე ყოველთვის, როცა სია შეიცვლება.
- **`pickCardFields`:** დეტალური გვერდის სერიალის ობიექტი დიდია (აღწერა, ყველა მსახიობი). სიის ობიექტიც ოცზე მეტ ველს შეიცავს. ვინახავთ მხოლოდ იმ ხუთ ველს, რომელიც ბარათს სჭირდება. `localStorage`-ს ლიმიტი აქვს, დაახლოებით 5 MB.
- **`setFavorites((current) => ...)`:** ფუნქციური ფორმა, რადგან ახალი სია ძველზეა დამოკიდებული.
- **`filter` და `[...current, x]`:** state-ს არასდროს ცვლი პირდაპირ (`push`, `splice`). ყოველთვის ახალ მასივს ქმნი, თორემ React ვერ გაიგებს, რომ რამე შეიცვალა.
- **`<FavoritesContext value={...}>`:** React 19-ის სინტაქსია. ძველ კოდში ნახავ `<FavoritesContext.Provider value={...}>`-ს, ეს იგივეა.

**ფაილი:** `src/hooks/useFavorites.js` (ახალი)

```js
import { useContext } from "react";
import { FavoritesContext } from "../context/FavoritesContext";

export function useFavorites() {
  const context = useContext(FavoritesContext);

  if (!context) {
    throw new Error(
      "useFavorites გამოიყენება მხოლოდ FavoritesProvider-ის შიგნით",
    );
  }

  return context;
}
```

**რატომ ეს hook:** კომპონენტებში წერ უბრალოდ `useFavorites()`-ს და არ გჭირდება ორი import. თუ ვინმე მას Provider-ის გარეთ გამოიყენებს, მიიღებს გასაგებ შეცდომას გაუგებარი `Cannot read properties of null`-ის ნაცვლად.

**რატომ სამი ფაილი:** Vite-ის Fast Refresh (კოდის შენახვისას გვერდი state-ის დაკარგვის გარეშე ახლდება) სწორად მუშაობს მხოლოდ მაშინ, როცა `.jsx` ფაილიდან მხოლოდ კომპონენტები გადის. ერთ ფაილში კომპონენტი და hook რომ ჩაგეწერა, `npm run lint` გაგაფრთხილებდა (`only-export-components`).

### 7.2. ჩართე Provider

**ფაილი:** `src/main.jsx` (დაემატა import და `<FavoritesProvider>`)

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router";
import App from "./App.jsx";
import FavoritesProvider from "./context/FavoritesProvider.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter useTransitions={false}>
      <FavoritesProvider>
        <App />
      </FavoritesProvider>
    </BrowserRouter>
  </StrictMode>,
);
```

`FavoritesProvider`-ის შიგნით მყოფ ყველა კომპონენტს შეუძლია `useFavorites()`-ის გამოძახება.

### 7.3. გულის ღილაკი

**ფაილი:** `src/components/FavoriteButton.jsx` (ახალი)

```jsx
import { useFavorites } from "../hooks/useFavorites";

export default function FavoriteButton({ show, className = "" }) {
  const { isFavorite, toggleFavorite } = useFavorites();
  const isActive = isFavorite(show.id);

  return (
    <button
      type="button"
      onClick={() => toggleFavorite(show)}
      aria-pressed={isActive}
      aria-label={`რჩეულებში დამატება: ${show.name}`}
      className={`grid size-10 place-items-center rounded-full bg-zinc-950/70 text-xl backdrop-blur transition hover:scale-110 ${
        isActive ? "text-amber-400" : "text-white"
      } ${className}`}
    >
      <span aria-hidden="true">{isActive ? "♥" : "♡"}</span>
    </button>
  );
}
```

- **`<button>` და არა `<div onClick>`:** ღილაკზე Tab-ით მიდიხარ და Enter / Space-ით აჭერ. `div`-ს ეს არ შეუძლია, და კლავიატურით მომხმარებელი ვერ გამოიყენებდა.
- **`aria-label`:** ღილაკში მხოლოდ სიმბოლოა. ტექსტის გარეშე ეკრანის წამკითხველი იტყოდა უბრალოდ „ღილაკი". სერიალის სახელიც შიგნითაა, თორემ 20 ერთნაირი ღილაკი იქნებოდა.
- **`aria-pressed`:** ეუბნება, რომ ეს გადამრთველია და ახლა ჩართულია თუ გამორთული.
- **`aria-hidden="true"` გულზე:** სიმბოლო დეკორაციაა, label უკვე ყველაფერს ამბობს.
- **`size-10` (40px):** თითით დასაჭერად ღილაკი საკმარისად დიდი უნდა იყოს.
- **`className = ""` prop:** გამომძახებელი წყვეტს, სად დადგეს ღილაკი. თვითონ ღილაკმა ეს არ იცის.

### 7.4. დაამატე ღილაკი ბარათზე

**ფაილი:** `src/components/ShowCard.jsx` (დაემატა import და `<FavoriteButton>` ბოლოში)

```jsx
import { Link } from "react-router";
import { formatRating, getYear } from "../utils/format";
import FavoriteButton from "./FavoriteButton";

export default function ShowCard({ show }) {
  const posterUrl = show.image?.medium;

  return (
    <article className="group relative">
      <Link to={`/show/${show.id}`} className="block rounded-lg">
        <div className="aspect-2/3 overflow-hidden rounded-lg bg-zinc-800">
          {posterUrl ? (
            <img
              src={posterUrl}
              alt=""
              loading="lazy"
              className="size-full object-cover transition duration-300 group-hover:scale-105"
            />
          ) : (
            <div className="flex size-full items-center justify-center p-4 text-center text-sm text-zinc-400">
              პოსტერი არ არის
            </div>
          )}
        </div>

        <h2 className="mt-2 line-clamp-1 font-medium">{show.name}</h2>
        <p className="text-sm text-zinc-400">
          {getYear(show.premiered)} · ★ {formatRating(show.rating.average)}
        </p>
      </Link>

      <FavoriteButton show={show} className="absolute top-2 right-2" />
    </article>
  );
}
```

**რატომ `Link`-ის გარეთ:** ღილაკი ლინკის შიგნით არასწორი HTML-ია (ინტერაქტიული ელემენტი ინტერაქტიულში), და დაჭერისას სერიალის გვერდზეც გადაგიყვანდა. ამიტომ ღილაკი `Link`-ის **მეზობელია**, და `absolute`-ით ვსვამთ პოსტერის კუთხეში. `article`-ს `relative` ამიტომ ჰქონდა თავიდანვე.

### 7.5. დაამატე ღილაკი დეტალურ გვერდზე

**ფაილი:** `src/pages/ShowDetailsPage.jsx` (ორი ცვლილება)

1. import-ებში დაამატე:

```jsx
import FavoriteButton from "../components/FavoriteButton";
```

2. იპოვე ხაზი `<h1 className="text-3xl font-bold">{show.name}</h1>` და ჩაანაცვლე ამით:

```jsx
          <div className="flex items-start justify-between gap-4">
            <h1 className="text-3xl font-bold">{show.name}</h1>
            <FavoriteButton show={show} className="shrink-0" />
          </div>
```

`shrink-0` ღილაკს არ აძლევს შეკუმშვის საშუალებას, როცა სათაური გრძელია.

### 7.6. რაოდენობა Header-ში

**ფაილი:** `src/components/Header.jsx` (დაემატა import, `useFavorites()` და რაოდენობა)

```jsx
import { Link, NavLink } from "react-router";
import { useFavorites } from "../hooks/useFavorites";

function getNavLinkClass({ isActive }) {
  return isActive ? "text-amber-400" : "text-zinc-300 hover:text-white";
}

export default function Header() {
  const { favorites } = useFavorites();

  return (
    <header className="sticky top-0 z-10 border-b border-zinc-800 bg-zinc-950/90 backdrop-blur">
      <div className="mx-auto flex max-w-6xl items-center justify-between px-4 py-4">
        <Link to="/" className="text-xl font-bold tracking-tight">
          Show<span className="text-amber-400">Search</span>
        </Link>

        <nav
          aria-label="მთავარი ნავიგაცია"
          className="flex gap-6 text-sm font-medium"
        >
          <NavLink to="/" end className={getNavLinkClass}>
            მთავარი
          </NavLink>
          <NavLink to="/favorites" className={getNavLinkClass}>
            რჩეულები ({favorites.length})
          </NavLink>
        </nav>
      </div>
    </header>
  );
}
```

### 7.7. რჩეულების გვერდი

**ფაილი:** `src/pages/FavoritesPage.jsx` (მთლიანად ჩაანაცვლე)

```jsx
import { Link } from "react-router";
import ShowGrid from "../components/ShowGrid";
import StatusMessage from "../components/StatusMessage";
import { useFavorites } from "../hooks/useFavorites";

export default function FavoritesPage() {
  const { favorites } = useFavorites();

  return (
    <>
      <title>რჩეულები · Show Search</title>
      <h1 className="mb-4 text-2xl font-bold">რჩეულები</h1>

      {favorites.length === 0 ? (
        <StatusMessage
          title="ჯერ არაფერი დაგიმატებია"
          description="სერიალის ბარათზე დააჭირე გულს და აქ გამოჩნდება."
        >
          <Link to="/" className="text-amber-400 underline underline-offset-4">
            სერიალების დათვალიერება
          </Link>
        </StatusMessage>
      ) : (
        <ShowGrid shows={favorites} />
      )}
    </>
  );
}
```

დააკვირდი: ახალი კომპონენტი არ დაგიწერია. `ShowGrid` და `StatusMessage` უკვე გქონდა. სწორად დაყოფილი კომპონენტების სარგებელი ეს არის.

### შეამოწმე

- გულზე დაჭერით ის ყვითლდება, Header-ში რიცხვი იზრდება, **გვერდი არ იცვლება**
- რჩეულების გვერდზე ჩანს დამატებული სერიალები
- განაახლე გვერდი: რჩეულები ადგილზეა
- DevTools → Application → Local Storage: ნახე, რა წერია
- წაშალე ყველა: ჩანს „ჯერ არაფერი დაგიმატებია"
- დეტალურ გვერდზეც მუშაობს, და იქ დამატებული მთავარზეც მონიშნულია
- კლავიატურით: Tab-ით მიდი გულზე, დააჭირე Enter

**Commit:** `feat: add favorites with context and localStorage`

---

## ეტაპი 8. გაპრიალება

**მიზანი:** პროექტი, რომლის ჩვენებაც არ შეგრცხვება.

### 8.1. კლავიატურის focus

**ფაილი:** `src/index.css` (მთლიანად ჩაანაცვლე)

```css
@import "tailwindcss";

@layer base {
  :focus-visible {
    outline: 2px solid var(--color-amber-400);
    outline-offset: 2px;
  }
}
```

**რატომ:** კლავიატურით მომხმარებელმა უნდა დაინახოს, სად არის. `:focus-visible` ჩარჩოს აჩვენებს მხოლოდ კლავიატურით გადაადგილებისას, მაუსის დაჭერისას არა. `outline: none`-ს ჩანაცვლების გარეშე არასდროს წერ, ეს accessibility-ის ყველაზე გავრცელებული შეცდომაა.

`var(--color-amber-400)`: Tailwind 4 ყველა ფერს CSS ცვლადად გაძლევს, ამიტომ ჩვეულებრივ CSS-შიც იმავე პალიტრას იყენებ.

### 8.2. შემოწმების სია

**რესპონსიულობა** (DevTools, `Ctrl+Shift+M`):

- [ ] 360px: 2 სვეტი, არაფერი სცდება, ჰორიზონტალური გადახვევა არ არის
- [ ] 768px: 4 სვეტი, დეტალურ გვერდზე ორი სვეტი
- [ ] 1280px: 5 სვეტი, შიგთავსი ცენტრშია

**კლავიატურა:**

- [ ] Tab-ით ყველა ლინკზე, ველზე და ღილაკზე მიდიხარ, ყვითელი ჩარჩო ჩანს
- [ ] თანმიმდევრობა ლოგიკურია: ლოგო → ნავიგაცია → ძებნა → ბარათები

**მდგომარეობები:**

- [ ] Offline რეჟიმში ჩანს შეცდომა და არა ცარიელი ეკრანი
- [ ] უაზრო ძებნაზე ჩანს „ვერაფერი მოიძებნა"
- [ ] უპოსტერო სერიალი ნორმალურად გამოიყურება (მოძებნე რამე იშვიათი)

**ხარისხი:**

- [ ] Console-ში შეცდომა და გაფრთხილება არ არის
- [ ] `npm run lint` სუფთაა
- [ ] `npx prettier --check .` სუფთაა (თუ არა: `npx prettier --write .`)
- [ ] კოდში არ დარჩა `console.log` და გამოუყენებელი import
- [ ] Lighthouse (DevTools → Lighthouse → Analyze): Accessibility 90+

**Lighthouse production ვერსიაზე გაუშვი**, development ბევრად ნელია:

```bash
npm run build
npm run preview
```

### 8.3. წვრილმანები

- `public/favicon.svg` ჩაანაცვლე საკუთარი ხატულით. Vite-ის ლოგო ჩანართში პირველი ნიშანია იმისა, რომ პროექტი ბოლომდე მიყვანილი არ არის.
- თუ ინტერფეისის ინგლისურად გაკეთება გინდა, ახლა შეცვალე ტექსტები და `index.html`-ში `lang="en"`. უცხოურ ვაკანსიებზე ინგლისური ინტერფეისი ჯობს.

**Commit:** `style: polish focus states and accessibility`

---

## ეტაპი 9. გაშვება და README

### 9.1. hosting-ის გამართვა

**ფაილი:** `vercel.json` (პროექტის ძირში, ახალი)

```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

**რატომ:** შენი საიტი რეალურად ერთი `index.html`-ია, routing-ს JavaScript აკეთებს. როცა ვინმე პირდაპირ ხსნის `/show/169`-ს, სერვერი ეძებს ასეთ ფაილს, ვერ პოულობს და 404-ს აბრუნებს. ეს წესი ეუბნება: ნებისმიერ მისამართზე გაეცი `index.html`. Netlify-ზე იგივეს აკეთებს ფაილი `public/_redirects` ერთი ხაზით: `/* /index.html 200`.

### 9.2. Deploy

1. `git push`
2. vercel.com → Add New → Project → აირჩიე შენი რეპოზიტორია
3. პარამეტრები უცვლელად დატოვე: Vercel თვითონ ხვდება, რომ ეს Vite პროექტია
4. Deploy

შემდეგ ცოცხალ ლინკზე გაიარე მთელი 8.2 სია თავიდან, **ტელეფონიდანაც**. აუცილებლად შეამოწმე პირდაპირი მისამართი: `შენი-საიტი.vercel.app/favorites`.

### 9.3. README

`README.md` რეკრუტერისთვის პროექტის სახეა. Vite-ის ნაგულისხმევი ტექსტი წაშალე და დაწერე საკუთარი. ნიმუში:

````markdown
# Show Search

სერიალების საძიებო აპლიკაცია TVmaze API-ზე: ძებნა, დეტალური გვერდი და რჩეულები.

**ცოცხალი ვერსია:** https://შენი-საიტი.vercel.app

![მთავარი გვერდი](./screenshots/home.png)

## ფუნქციონალი

- ტოპ სერიალების სია და ძებნა (debounce-ით)
- ძებნის ტექსტი URL-ში: ლინკის გაზიარება და გვერდის განახლება შედეგს ინარჩუნებს
- სერიალის დეტალური გვერდი მსახიობებით
- რჩეულები, რომლებიც localStorage-ში ინახება
- loading, error და ცარიელი მდგომარეობები
- რესპონსიული დიზაინი და კლავიატურით მართვა

## ტექნოლოგიები

React 19, React Router 8, Tailwind CSS 4, Vite

## ლოკალურად გაშვება

```bash
git clone https://github.com/შენი_სახელი/show-search.git
cd show-search
npm install
npm run dev
```

## რა ვისწავლე

აქ დაწერე 3-4 წინადადება **შენი სიტყვებით**. მაგალითად: რა იყო race condition
ძებნაში და როგორ მოაგვარე AbortController-ით; რატომ შეინახე ძებნის ტექსტი
URL-ში და არა state-ში; რატომ გაასუფთავე API-დან მოსული HTML და როგორ.

---

მონაცემების წყარო: [TVmaze](https://www.tvmaze.com) (CC BY-SA)
````

სქრინშოტები (დესკტოპი და მობილური) შეინახე `screenshots` საქაღალდეში.

**Commit:** `docs: add readme and deploy config`

---

## რას გკითხავენ ამ პროექტზე გასაუბრებაზე

სცადე თითოეულს ხმამაღლა, ფაილში ჩაუხედავად უპასუხო. პასუხები ამავე ფაილშია, შესაბამის ეტაპზე.

1. რა არის debounce და რატომ გამოიყენე? (5.1)
2. რა არის race condition და როგორ აიცილე? (4.6, 5 „შეამოწმე")
3. რატომ ეშვება effect ორჯერ development-ში? (ეტაპი 2)
4. რას აკეთებს effect-ის მიერ დაბრუნებული ფუნქცია? (4.6)
5. რატომ ინახავ ძებნის ტექსტს URL-ში? (5.3)
6. რას აკეთებს `key` და რატომ არ იყენებ index-ს? (4.5, 5.3)
7. რა არის Context და როდის გჭირდება? (7.1)
8. რატომ `status` ერთი სტრიქონით და არა ორი boolean? (4.6)
9. რატომ ამოწმებ `response.ok`-ს? (ეტაპი 2)
10. რატომ არ გამოიყენე `dangerouslySetInnerHTML` აღწერისთვის? (6.1)
11. რატომ არის გულის ღილაკი `Link`-ის გარეთ? (7.4)
12. რით ჯობს `Link` ჩვეულებრივ `<a>`-ს? (3.3)

## შემდეგი ნაბიჯები (სურვილისამებრ)

MVP მზადაა. თუ დრო გრჩება, თითო-თითოდ დაამატე:

- **„მეტის ჩატვირთვა" ღილაკი.** `/shows?page=0` 250 სერიალს აბრუნებს, შენ კი 20-ს აჩვენებ. დაამატე ღილაკი, რომელიც კიდევ 20-ს აჩენს. ამისთვის `getTopShows`-მა მთელი დახარისხებული სია უნდა დააბრუნოს, `slice` კი კომპონენტში გადაიტანე. ახალი მოთხოვნა არ გჭირდება.
- **ჟანრის ფილტრი.** იმავე 250-იან სიაზე `genres` ველით გაფილტრე.
- **ეპიზოდების სია დეტალურ გვერდზე.** endpoint: `/shows/:id/episodes`.
- **ტესტები.** `src/utils/format.js` იდეალური საწყისია: დააყენე Vitest და დაწერე 5-6 ტესტი.

შემდეგ გადადი მაღაზიის პროექტზე. იქ გამოგადგება ამ პროექტის `useDebounce`, `Button`, `StatusMessage`, Context-ის ნიმუში და მთელი API ფენის სტრუქტურა.

## დოკუმენტაცია

- React: https://react.dev/learn
- React Router (Declarative mode): https://reactrouter.com/start/declarative/installation
- Tailwind CSS: https://tailwindcss.com/docs
- TVmaze API: https://www.tvmaze.com/api
- Vite: https://vite.dev/guide/
