# Vue Weather App

[Visit live demo here](https://zpipe07.github.io/vue-weather/)

## Notes

Since I haven't used Vue.js in several years, I had to reacquaint myself with it. I found it to be very intuitive.

There were some things that I didn't get to implement in the time limit:

- Move data fetching outside of `<WeatherInfo />` component into a composable.
- Cache data responses; it probably isn't necessary to refetch data when navigating between tabs.
- Improve icon and description determination for "Next 5 days" component; the current implementation sets the icon and description based on the first hourly instance of the day which is ok, but could be improved.

## Run the project

```sh
# Install dependencies
npm install

# Compile and Hot-Reload for Development
npm run dev

# Type-Check, Compile and Minify for Production
npm run build

# Lint with ESLint
npm run lint
```
