# URL Shortener Frontend Application

This is a client-side URL shortener application built with React, TypeScript, and Material-UI. It allows users to create shortened URLs, view usage statistics, and be redirected, all within the browser using `localStorage` for data persistence. The application also integrates a mandatory remote logging service to track key events.

## Features

-   **Bulk URL Shortening**: Create up to 5 shortened URLs at once.
-   **Customization**:
    -   Option to provide a custom alphanumeric shortcode (4-10 characters).
    -   Set a custom expiration time for each link (in minutes).
-   **Statistics Dashboard**:
    -   View all created short links in a sortable table.
    -   See the original URL, creation date, expiration date, and total click count.
    -   Expandable rows to view detailed click history (timestamp, source, location).
-   **Client-Side Redirection**: Shortened links (`/your-shortcode`) are automatically handled and redirected to the original long URL.
-   **Data Persistence**: All URL data and click history are stored in the browser's `localStorage`.
-   **Remote Logging**: All major user actions and application events are logged to a remote evaluation service.
-   **Responsive Design**: The interface is built with Material-UI for a clean and responsive experience on both desktop and mobile devices.

## Tech Stack

-   **Framework**: [React](https://reactjs.org/)
-   **Build Tool**: [Vite](https://vitejs.dev/)
-   **Language**: [TypeScript](https://www.typescriptlang.org/)
-   **UI Library**: [Material-UI](https://mui.com/)
-   **Routing**: [React Router](https://reactrouter.com/)
-   **HTTP Client**: [Axios](https://axios-http.com/) (for the logging service)
-   **Utilities**:
    -   [date-fns](https://date-fns.org/) for date/time formatting.
    -   [nanoid](https://github.com/ai/nanoid) for generating unique shortcodes.

## Project Structure

The project follows a standard React application structure, organized for clarity and scalability.

```
/src
|-- components/       # Reusable React components (e.g., RedirectHandler)
|-- lib/              # Core libraries, including the logging service
|   |-- logging/
|       |-- logger.ts # Mandatory logging implementation
|-- pages/            # Top-level page components for routes
|   |-- UrlShortenerPage.tsx
|   |-- UrlStatisticsPage.tsx
|-- services/         # Business logic and data management
|   |-- urlService.ts # Handles all interactions with localStorage
|-- types/            # TypeScript type definitions
|   |-- url.types.ts
|-- App.tsx           # Main application component with routing and layout
|-- main.tsx          # Application entry point
```

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine.

### Prerequisites

-   [Node.js](https://nodejs.org/) (v18 or later recommended)
-   A package manager like [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)

### Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd frontend-test-submission
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Configure Environment Variables:**

    The application requires an API token to communicate with the remote logging service.

    -   Create a new file named `.env.local` in the root of the project directory.
    -   Add the following line to the file, replacing `YOUR_API_KEY_HERE` with your actual token:

    ```env
    VITE_API_AUTH_TOKEN="YOUR_API_KEY_HERE"
    ```

    > **Note:** The `VITE_` prefix is required by Vite to expose the variable to the client-side code.

### Available Scripts

-   **Run the development server:**
    This will start the app on `http://localhost:5173`.
    ```bash
    npm run dev
    ```

-   **Build for production:**
    This will create a `dist` folder with the optimized production build.
    ```bash
    npm run build
    ```

-   **Lint the code:**
    This will run ESLint to check for code quality and style issues.
    ```bash
    npm run lint
    ```

-   **Preview the production build:**
    This command serves the `dist` folder locally to test the production build.
    ```bash
    npm run preview
    ```

## How It Works

### Data Persistence

The application is designed to be fully client-side. All data, including the list of shortened URLs and their associated click history, is stored in the browser's `localStorage`. The `urlService.ts` module acts as a repository, abstracting all read/write operations to `localStorage`. This means your data will persist across browser sessions but will not be available on other devices or in other browsers.

### Routing

`react-router-dom` is used to manage navigation and application views:

-   `/`: The main page for creating new short URLs (`UrlShortenerPage`).
-   `/stats`: The dashboard for viewing statistics of all created URLs (`UrlStatisticsPage`).
-   `/:shortcode`: A dynamic route handled by the `RedirectHandler` component. It looks up the shortcode, validates it, records a click, and performs a redirect to the original URL.
-   A wildcard route `*` handles 404 (Not Found) errors within the main application layout.
