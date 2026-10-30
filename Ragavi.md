package com.recipe.smart_recipe_backend;

import org.springframework.web.bind.annotation.*;
import java.util.*;

@RestController
@CrossOrigin(origins = "*")
public class RecipeController {

    @GetMapping("/api/recipes")
    public List<Map<String, Object>> getRecipes(
            @RequestParam(defaultValue = "") String ingredients) {

        List<Map<String, Object>> recipes = new ArrayList<>();

        addRecipe(recipes, "Vegetable Fried Rice",
                "rice, carrot, onion, peas",
                List.of(
                    "Cook the rice and keep it aside.",
                    "Wash and chop the carrot and onion.",
                    "Heat oil in a pan and saute the onion.",
                    "Add carrot and peas and cook until tender.",
                    "Add the cooked rice and mix everything well.",
                    "Add salt to taste, stir and serve hot."
                ), ingredients);

        addRecipe(recipes, "Tomato Pasta",
                "pasta, tomato, onion, garlic",
                List.of(
                    "Boil pasta in water until tender, then drain it.",
                    "Chop the tomatoes, onion and garlic.",
                    "Heat oil in a pan and saute the onion and garlic.",
                    "Add tomatoes and cook until they become soft.",
                    "Add salt and your preferred seasonings.",
                    "Mix in the cooked pasta and serve hot."
                ), ingredients);

        addRecipe(recipes, "Potato Sandwich",
                "potato, bread, onion",
                List.of(
                    "Wash the potatoes and boil them until soft.",
                    "Allow them to cool slightly, then peel and mash them.",
                    "Chop the onion and mix it with the mashed potato.",
                    "Add salt and seasonings to taste.",
                    "Spread the potato mixture onto a bread slice.",
                    "Cover with another slice and toast until golden."
                ), ingredients);

        addRecipe(recipes, "Omelette",
                "egg, onion, tomato",
                List.of(
                    "Crack the eggs into a bowl.",
                    "Chop the onion and tomato into small pieces.",
                    "Add the vegetables, salt and seasonings to the eggs.",
                    "Beat the mixture until well combined.",
                    "Heat a lightly oiled pan over medium heat.",
                    "Pour in the mixture and cook until set.",
                    "Flip carefully if needed, cook through and serve."
                ), ingredients);

        return recipes;
    }

    private void addRecipe(
            List<Map<String, Object>> recipes,
            String name,
            String recipeIngredients,
            List<String> steps,
            String userIngredients) {

        if (userIngredients.isBlank()) {
            return;
        }

        List<String> available = Arrays.stream(
                userIngredients.toLowerCase().split("[, ]+"))
                .filter(s -> !s.isBlank())
                .toList();

        long matches = Arrays.stream(
                recipeIngredients.split(", "))
                .filter(item -> available.contains(item))
                .count();

        if (matches > 0) {
            Map<String, Object> recipe = new LinkedHashMap<>();
            recipe.put("name", name);
            recipe.put("ingredients", recipeIngredients);
            recipe.put("instructions", String.join(" ", steps));
            recipe.put("steps", steps);
            recipe.put("matchedIngredients", matches);
            recipes.add(recipe);
        }
    }
}
