#include <iostream>
#include <SDL2/SDL.h>

const int WIDTH = 800, HEIGHT = 600;

int factorial(int n) {
    if (n == 0 || n == 1) {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}

void pascalsTriangle(int numRows) {
    for (int i = 0; i < numRows; i++) {
        for (int j = 0; j <= i; j++) {
            int element = factorial(i) / (factorial(j) * factorial(i - j));
            std::cout << element << " ";
        }
        std::cout << std::endl;
    }
}

int main(int argc, char *argv[]) {
    if (SDL_Init(SDL_INIT_EVERYTHING)!= 0) {
        std::cout << "SDL_Init() failed: " << SDL_GetError() << std::endl;
        return 1;
    }

    SDL_Window *window = SDL_CreateWindow("Hello SDL WORLD", SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, WIDTH, HEIGHT, SDL_WINDOW_ALLOW_HIGHDPI);

    if (NULL == window) {
        std::cout << "Could not create window:" << SDL_GetError() << std::endl;
        SDL_Quit();
        return 1;
    }

    SDL_Renderer *renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    if (NULL == renderer) {
        std::cout << "Could not create renderer:" << SDL_GetError() << std::endl;
        SDL_DestroyWindow(window);
        SDL_Quit();
        return 1;
    }

    int numRows;

    std::cout << "Enter the number of rows for Pascal's Triangle: ";
    std::cin >> numRows;

    pascalsTriangle(numRows);

    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);

    for (int i = 0; i < numRows; i++) {
        for (int j = 0; j <= i; j++) {
            int element = factorial(i) / (factorial(j) * factorial(i - j));
            SDL_Rect square = {WIDTH / 2 - numRows / 2 + j * 10, HEIGHT / 2 - i * 10, 10, 10};
            SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
            SDL_RenderFillRect(renderer, &square);
        }
    }

    SDL_RenderPresent(renderer);

    SDL_Event windowEvent;
    bool quit = false;

    while (!quit) {
        while (SDL_PollEvent(&windowEvent)) {
            if (SDL_QUIT == windowEvent.type) {
                quit = true;
            }
        }
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return EXIT_SUCCESS;
}
