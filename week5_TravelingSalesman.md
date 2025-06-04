### Traveling Salesman Problem (TSP)

The Traveling Salesman Problem (TSP) is a classic optimization problem in computer science and operations research. It is one of the most intensively studied problems in optimization due to its complexity and wide range of applications.

#### Problem Definition

The TSP can be described as follows:
- Given a list of cities and the distances between each pair of cities, the task is to find the shortest possible route that visits each city exactly once and returns to the origin city.

#### Example

Imagine a salesman who needs to visit a number of cities. The salesman starts at a specific city, visits each city exactly once, and returns to the starting city. The goal is to minimize the total distance traveled.

#### Applications

The TSP has many practical applications, including:
- **Logistics and transportation**: Optimizing delivery routes for goods and services.
- **Manufacturing**: Planning the movement of machines or tools in a factory.
- **Circuit design**: Minimizing the length of wiring in electronic circuits.

#### Complexity

The TSP is known to be NP-hard, meaning that there is no known polynomial-time algorithm to solve it for all instances. The number of possible routes grows factorially with the number of cities, leading to a time complexity of O(n!), where n is the number of cities.

#### Naive Approach

A naive approach to solving the TSP is to generate all possible permutations of the cities and calculate the total distance for each permutation. The permutation with the shortest distance is the optimal solution. However, this approach is impractical for large numbers of cities due to its factorial time complexity.

Here is a Java implementation of the naive approach to solve the TSP:

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class TravelingSalesmanProblem {
    public static void main(String[] args) {
        int[][] distanceMatrix = {
            {0, 10, 15, 20},
            {10, 0, 35, 25},
            {15, 35, 0, 30},
            {20, 25, 30, 0}
        };
        List<Integer> cities = Arrays.asList(0, 1, 2, 3);
        List<Integer> bestRoute = new ArrayList<>();
        int[] minDistance = {Integer.MAX_VALUE};

        permute(cities, 0, distanceMatrix, bestRoute, minDistance);

        System.out.println("Shortest route: " + bestRoute);
        System.out.println("Minimum distance: " + minDistance[0]);
    }

    public static void permute(List<Integer> cities, int start, int[][] distanceMatrix, List<Integer> bestRoute, int[] minDistance) {
        if (start == cities.size() - 1) {
            int currentDistance = calculateTotalDistance(cities, distanceMatrix);
            if (currentDistance < minDistance[0]) {
                minDistance[0] = currentDistance;
                bestRoute.clear();
                bestRoute.addAll(cities);
            }
        } else {
            for (int i = start; i < cities.size(); i++) {
                swap(cities, start, i);
                permute(cities, start + 1, distanceMatrix, bestRoute, minDistance);
                swap(cities, start, i);
            }
        }
    }

    public static void swap(List<Integer> cities, int i, int j) {
        int temp = cities.get(i);
        cities.set(i, cities.get(j));
        cities.set(j, temp);
    }

    public static int calculateTotalDistance(List<Integer> cities, int[][] distanceMatrix) {
        int totalDistance = 0;
        for (int i = 0; i < cities.size() - 1; i++) {
            totalDistance += distanceMatrix[cities.get(i)][cities.get(i + 1)];
        }
        totalDistance += distanceMatrix[cities.get(cities.size() - 1)][cities.get(0)];
        return totalDistance;
    }
}
```

#### Explanation

1. **Distance Matrix**: The `distanceMatrix` represents the distances between each pair of cities.
2. **Permutations**: The `permute` function generates all possible permutations of the cities.
3. **Distance Calculation**: The `calculateTotalDistance` function calculates the total distance for a given permutation of cities.
4. **Optimal Route**: The algorithm keeps track of the permutation with the shortest distance and updates the `bestRoute` and `minDistance` accordingly.

#### Conclusion

The naive approach to solving the TSP is computationally expensive and impractical for large numbers of cities due to its factorial time complexity. However, it serves as a good example of the challenges posed by NP-hard problems and the need for more efficient algorithms and heuristics to solve such problems in practice.


| From/To          | Chicago, IL | Los Angeles, CA | New York, NY | Phoenix, AZ | Boston, MA | Miami, FL | Philadelphia, PA | Seattle, WA | Atlanta, GA | Denver, CO |
|------------------|-------------|-----------------|--------------|-------------|------------|-----------|------------------|-------------|-------------|------------|
| **Chicago, IL**  | -           | 2,015 miles     | 790 miles    | 1,750 miles | 980 miles  | 1,380 miles | 760 miles        | 2,060 miles | 715 miles   | 1,000 miles|
| **Los Angeles, CA** | 2,015 miles | -             | 2,780 miles  | 370 miles   | 2,990 miles | 2,730 miles | 2,720 miles      | 1,130 miles | 2,180 miles | 1,010 miles|
| **New York, NY** | 790 miles   | 2,780 miles     | -            | 2,460 miles | 215 miles  | 1,280 miles | 95 miles         | 2,860 miles | 860 miles   | 1,800 miles|
| **Phoenix, AZ**  | 1,750 miles | 370 miles       | 2,460 miles  | -           | 2,890 miles | 2,330 miles | 2,300 miles      | 1,420 miles | 1,845 miles | 850 miles  |
| **Boston, MA**   | 980 miles   | 2,990 miles     | 215 miles    | 2,890 miles | -          | 1,490 miles | 310 miles        | 3,070 miles | 1,075 miles | 1,950 miles|
| **Miami, FL**    | 1,380 miles | 2,730 miles     | 1,280 miles  | 2,330 miles | 1,490 miles | -         | 1,220 miles      | 3,280 miles | 660 miles   | 2,060 miles|
| **Philadelphia, PA** | 760 miles | 2,720 miles   | 95 miles     | 2,300 miles | 310 miles  | 1,220 miles | -                | 2,870 miles | 800 miles   | 1,760 miles|
| **Seattle, WA**  | 2,060 miles | 1,130 miles     | 2,860 miles  | 1,420 miles | 3,070 miles | 3,280 miles | 2,870 miles      | -           | 2,640 miles | 1,300 miles|
| **Atlanta, GA**  | 715 miles   | 2,180 miles     | 860 miles    | 1,845 miles | 1,075 miles | 660 miles  | 800 miles        | 2,640 miles | -           | 1,400 miles|
| **Denver, CO**   | 1,000 miles | 1,010 miles     | 1,800 miles  | 850 miles   | 1,950 miles | 2,060 miles | 1,760 miles      | 1,300 miles | 1,400 miles | -          |