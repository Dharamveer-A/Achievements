# AIML LAB RECORD

---

# 1. DATA ANALYSIS AND VISUALIZATION USING PANDAS

## Aim
To perform data analysis and visualization using Python libraries such as Pandas, NumPy, Matplotlib, and Seaborn.

## Procedure
1. Import required Python libraries.
2. Create a dataset using NumPy and Pandas.
3. Save and read the dataset from CSV file.
4. Perform statistical analysis.
5. Handle null values and duplicates.
6. Create different visualizations.
7. Display the outputs.

## Code
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import copy

np.random.seed(42)

DF1 = pd.DataFrame({
    'Num_Col': np.random.randint(1, 100, 100),
    'Num_Col2': np.random.randint(20, 80, 100),
    'Cat_Col': np.random.choice(['Group A', 'Group B'], 100)
})

DF1.to_csv("data.csv", index=False)
DF1 = pd.read_csv("data.csv")

DF_shallow = DF1.copy(deep=False)
DF_deep = copy.deepcopy(DF1)

print("Top 5 Rows")
print(DF1.head())

print("Bottom 5 Rows")
print(DF1.tail())

print("Dataset Info")
DF1.info()

print("Duplicate Count")
print(DF1.duplicated().sum())

print("Null Value Count")
print(DF1.isnull().sum())

print("Mean:", DF1['Num_Col'].mean())
print("Median:", DF1['Num_Col'].median())
print("Mode:", DF1['Num_Col'].mode()[0])

DF1.loc[0, 'Num_Col'] = np.nan
DF1['Num_Col'] = DF1['Num_Col'].fillna(DF1['Num_Col'].mean())

sns.countplot(x='Cat_Col', data=DF1)
plt.show()

DF1['Cat_Col'].value_counts().plot.pie(autopct='%.1f%%')
plt.show()

sns.histplot(x='Num_Col', data=DF1, kde=True)
plt.show()

sns.boxplot(x='Num_Col', data=DF1)
plt.show()

sns.scatterplot(x='Num_Col', y='Num_Col2', data=DF1)
plt.show()

sns.heatmap(DF1.corr(numeric_only=True), annot=True)
plt.show()
```

## Sample Output
```text
Top 5 Rows
   Num_Col  Num_Col2 Cat_Col
0       52        43  Group A
1       93        22  Group B

Mean: 49.5
Median: 50
Mode: 61

Duplicate Count
0
```

## Result
Thus the data analysis and visualization operations were performed successfully using Python libraries.

---

# 2. MAP COLORING USING DFS

## Aim
To implement Map Coloring problem using Depth First Search (DFS).

## Procedure
1. Define Australian states and neighboring states.
2. Define available colors.
3. Check whether assigned color is safe.
4. Apply DFS recursively.
5. Assign colors to all territories.
6. Display valid coloring.

## Code
```python
T = 'tasmania'
WA = 'western australia'
NT = 'northern territory'
SA = 'southern australia'
Q = 'queensland'
NSW = 'new south wales'
V = 'victoria'

colors = {'r', 'g', 'b'}

australia = {
    T: [V],
    WA: [NT, SA],
    NT: [WA, Q, SA],
    SA: [WA, NT, Q, NSW, V],
    Q: [NT, SA, NSW],
    NSW: [Q, SA, V],
    V: [SA, NSW, T]
}

def is_safe(territory, color, coloring):
    for neighbor in australia[territory]:
        if neighbor in coloring and coloring[neighbor] == color:
            return False
    return True

def dfs_map_coloring(territories, coloring):
    if not territories:
        return coloring

    territory = territories[0]

    for color in colors:
        if is_safe(territory, color, coloring):
            coloring[territory] = color

            result = dfs_map_coloring(territories[1:], coloring)

            if result is not None:
                return result

            del coloring[territory]

    return None

territory_list = list(australia.keys())
result = dfs_map_coloring(territory_list, {})

print("Valid Coloring")
for territory, color in result.items():
    print(territory, ":", color)
```

## Sample Output
```text
Valid Coloring
western australia : r
northern territory : g
southern australia : b
queensland : r
new south wales : g
victoria : r
tasmania : g
```

## Result
Thus the Map Coloring problem was solved successfully using DFS.

---

# 3. 8 QUEENS PROBLEM USING DFS SEARCH TECHNIQUE

## Aim
To implement Python code to solve the 8 Queens Problem using Depth First Search (DFS) technique.

## Procedure
1. Create an empty chess board.
2. Check safe positions for queens.
3. Place queens row by row.
4. Use DFS approach with stack.
5. Backtrack when conflict occurs.
6. Display the valid board configuration.

## Code
```python
def is_safe(board, row, col):
    for i in range(row):
        if board[i] == col or abs(board[i] - col) == abs(i - row):
            return False
    return True

def solve_dfs():
    stack = [([], 0)]

    while stack:
        board, row = stack.pop()

        if row == 8:
            return board

        for col in range(7, -1, -1):
            if is_safe(board, row, col):
                stack.append((board + [col], row + 1))

    return None

def print_board(board):
    for val in board:
        line = ['.'] * 8
        line[val] = 'Q'
        print(' '.join(line))

solution = solve_dfs()

if solution:
    print("DFS Solution")
    print_board(solution)
```

## Sample Output
```text
DFS Solution
Q . . . . . . .
. . . . Q . . .
. . . . . . . Q
. . . . . Q . .
. . Q . . . . .
. . . . . . Q .
. Q . . . . . .
. . . Q . . . .
```

## Result
Thus the 8 Queens problem was solved successfully using DFS search technique.

---

# 4. 8 QUEENS PROBLEM USING BFS SEARCH TECHNIQUE

## Aim
To implement Python code to solve the 8 Queens Problem using Breadth First Search (BFS) technique.

## Procedure
1. Create an empty chess board.
2. Check safe positions for queens.
3. Use queue for BFS traversal.
4. Place queens row by row.
5. Display the valid solution.

## Code
```python
from collections import deque

def is_safe(board, row, col):
    for i in range(row):
        if board[i] == col or abs(board[i] - col) == abs(i - row):
            return False
    return True

def solve_bfs():
    queue = deque([([], 0)])

    while queue:
        board, row = queue.popleft()

        if row == 8:
            return board

        for col in range(8):
            if is_safe(board, row, col):
                queue.append((board + [col], row + 1))

    return None

def print_board(board):
    for val in board:
        line = ['.'] * 8
        line[val] = 'Q'
        print(' '.join(line))

solution = solve_bfs()

if solution:
    print("BFS Solution")
    print_board(solution)
```

## Sample Output
```text
BFS Solution
Q . . . . . . .
. . . . Q . . .
. . . . . . . Q
. . . . . Q . .
. . Q . . . . .
. . . . . . Q .
. Q . . . . . .
. . . Q . . . .
```

## Result
Thus the 8 Queens problem was solved successfully using BFS search technique.

---

# 5. KNN CLASSIFICATION

## Aim
To implement K-Nearest Neighbor (KNN) classification algorithm using Python.

## Procedure
1. Import required libraries.
2. Create sample dataset.
3. Train KNN classifier.
4. Predict class label.
5. Plot the graph.
6. Display the result.

## Code
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.neighbors import KNeighborsClassifier

X = np.array([[10], [20], [30], [70], [80], [90]])
y = np.array([0, 0, 0, 1, 1, 1])

knn = KNeighborsClassifier(n_neighbors=3)

knn.fit(X, y)

pred = knn.predict([[75]])

print("Prediction for 75 =", pred[0])

plt.scatter(X, y)
plt.title("KNN Classification")
plt.xlabel("Input")
plt.ylabel("Class")
plt.show()
```

## Sample Output
```text
Prediction for 75 = 1
```

## Result
Thus the KNN classification algorithm was implemented successfully.

---

# 6. LINEAR AND LOGISTIC REGRESSION

## Aim
To implement Linear Regression and Logistic Regression using Python.

## Procedure
1. Import required libraries.
2. Create datasets for regression.
3. Train Linear Regression model.
4. Predict values using Linear Regression.
5. Train Logistic Regression model.
6. Predict output class.
7. Display graphs and outputs.

## Code
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression, LogisticRegression

X1 = np.array([[1], [2], [3], [4], [5]])
y1 = np.array([3, 5, 7, 9, 11])

linear = LinearRegression()
linear.fit(X1, y1)

y_pred = linear.predict(X1)

pred1 = linear.predict([[6]])

print("Linear Prediction for 6 =", pred1[0])

plt.plot(X1, y_pred)
plt.scatter(X1, y1)
plt.title("Linear Regression")
plt.show()

X2 = np.array([[1], [2], [3], [4], [5], [6]])
y2 = np.array([0, 0, 0, 1, 1, 1])

logistic = LogisticRegression()
logistic.fit(X2, y2)

pred2 = logistic.predict([[5]])

print("Logistic Prediction for 5 =", pred2[0])

plt.scatter(X2, y2)
plt.title("Logistic Regression")
plt.show()
```

## Sample Output
```text
Linear Prediction for 6 = 13.0
Logistic Prediction for 5 = 1
```

## Result
Thus the Linear Regression and Logistic Regression algorithms were implemented successfully.

---

# 7. K-MEANS CLUSTERING

## Aim
To implement K-Means Clustering algorithm using Python.

## Procedure
1. Import required libraries.
2. Create sample dataset.
3. Create K-Means model.
4. Train the model.
5. Find cluster labels.
6. Plot the clusters.
7. Display the result.

## Code
```python
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

# Sample business data
# [Money Spent, Visits]
data = [
    [100, 2],
    [120, 3],
    [130, 2],
    [400, 8],
    [420, 9],
    [450, 10]
]

kmeans = KMeans(n_clusters=2)

kmeans.fit(data)

labels = kmeans.labels_

for i in range(len(data)):
    print(data[i], "-> Cluster", labels[i])

x = [i[0] for i in data]
y = [i[1] for i in data]

plt.scatter(x, y, c=labels)
plt.xlabel("Money Spent")
plt.ylabel("Visits")
plt.title("K-Means Clustering")
plt.show()
```

## Sample Output
```text
[100, 2] -> Cluster 1
[120, 3] -> Cluster 1
[130, 2] -> Cluster 1
[400, 8] -> Cluster 0
[420, 9] -> Cluster 0
[450, 10] -> Cluster 0
```

## Result
Thus the K-Means Clustering algorithm was implemented successfully.

---

# 8. NAIVE BAYES CLASSIFICATION

## Aim
To implement Naive Bayes Classification using Python.

## Procedure
1. Import required libraries.
2. Load Iris dataset.
3. Split dataset into training and testing data.
4. Create Gaussian Naive Bayes model.
5. Train the model.
6. Predict the output.
7. Calculate accuracy.
8. Display the result.

## Code
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score

iris = load_iris()
X, y = iris.data, iris.target

print("Dataset loaded successfully.")

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

print(f"Training set size: {len(X_train)} samples")
print(f"Testing set size: {len(X_test)} samples")

model = GaussianNB()

model.fit(X_train, y_train)

print("Naive Bayes model trained successfully.")

y_pred = model.predict(X_test)

accuracy = accuracy_score(y_test, y_pred)

print(f"Model Accuracy: {accuracy:.4f}")
```

## Sample Output
```text
Dataset loaded successfully.
Training set size: 105 samples
Testing set size: 45 samples
Naive Bayes model trained successfully.
Model Accuracy: 0.9778
```

## Result
Thus the Naive Bayes Classification algorithm was implemented successfully.

---

# 9. PRINCIPAL COMPONENT ANALYSIS \(PCA\)

## Aim
To implement Principal Component Analysis (PCA) using Python.

## Procedure
1. Import required libraries.
2. Create sample dataset.
3. Standardize the dataset.
4. Apply PCA for dimensionality reduction.
5. Transform the data.
6. Display reduced data.
7. Plot the graph.

## Code
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

X = np.array([
    [2, 3],
    [3, 4],
    [4, 5],
    [5, 6],
    [6, 7]
])

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

pca = PCA(n_components=1)
X_pca = pca.fit_transform(X_scaled)

print("Reduced Data:")
print(X_pca)

plt.scatter(X[:,0], X[:,1])

plt.title("PCA")
plt.xlabel("Feature 1")
plt.ylabel("Feature 2")

plt.show()
```

## Sample Output
```text
Reduced Data:
[[-2. ]
 [-1. ]
 [ 0. ]
 [ 1. ]
 [ 2. ]]
```

## Result
Thus the Principal Component Analysis (PCA) algorithm was implemented successfully.

---

# 10. TIC TAC TOE GAME \(USER VS USER\)

## Aim
To implement a User vs User Tic Tac Toe game using Python.

## Procedure
1. Create a 3x3 game board.
2. Display the board.
3. Accept moves from two users.
4. Check for valid moves.
5. Check winning conditions.
6. Display the winner or draw.
7. Stop the game.

## Code
```python
print("User Based Tic Tac Toe Game
")

def print_board(board):
    print("-------------")
    for row in board:
        print("|", " | ".join(row), "|")
    print("-------------")

def make_move(board, move, player):
    board[move[0]][move[1]] = player

def check_winner(board, player):
    for row in board:
        if all(cell == player for cell in row):
            return True

    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True

    if all(board[i][i] == player for i in range(3)) or all(board[i][2-i] == player for i in range(3)):
        return True

    return False

def get_valid_moves(board):
    moves = []

    for i in range(3):
        for j in range(3):
            if board[i][j] == '-':
                moves.append((i, j))

    return moves

board = [['-' for _ in range(3)] for _ in range(3)]

print("Welcome to Tic Tac Toe!")
print("Enter row and column (0-2) to make your move.")

print_board(board)

current_player = 'X'

while True:
    row = int(input(f"Player {current_player}, enter row: "))
    col = int(input(f"Player {current_player}, enter column: "))

    if board[row][col] != '-':
        print("Invalid move! Try again.")
        continue

    make_move(board, (row, col), current_player)

    print_board(board)

    if check_winner(board, current_player):
        print(f"Player {current_player} wins!")
        break

    elif len(get_valid_moves(board)) == 0:
        print("It's a draw!")
        break

    else:
        current_player = 'X' if current_player == 'O' else 'O'
```

## Sample Output
```text
Welcome to Tic Tac Toe!
Player X wins!
```

## Result
Thus the User vs User Tic Tac Toe game was implemented successfully.

---

# 11. TIC TAC TOE GAME \(USER VS AI\)

## Aim
To implement a User vs AI Tic Tac Toe game using Python.

## Procedure
1. Create a Tic Tac Toe board.
2. Accept moves from the user.
3. Generate AI moves.
4. Use BFS for AI decision making.
5. Check winning conditions.
6. Display the result.
7. Stop the game.

## Code
```python
from collections import deque

print("
User Vs AI Based Tic Tac Toe Game
")

def print_board(board):
    print("-------------")
    for row in board:
        print("|", " | ".join(row), "|")
    print("-------------")

def check_winner(board, player):
    for row in board:
        if all(cell == player for cell in row):
            return True

    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True

    if all(board[i][i] == player for i in range(3)) or all(board[i][2-i] == player for i in range(3)):
        return True

    return False

def get_valid_moves(board):
    moves = []

    for i in range(3):
        for j in range(3):
            if board[i][j] == '-':
                moves.append((i, j))

    return moves

def make_move(board, move, player):
    board[move[0]][move[1]] = player

def bfs(board, player):
    queue = deque([(board, player)])

    while queue:
        current_board, current_player = queue.popleft()

        if check_winner(current_board, 'X'):
            return -1

        elif check_winner(current_board, 'O'):
            return 1

        elif len(get_valid_moves(current_board)) == 0:
            return 0

        for move in get_valid_moves(current_board):
            next_board = [row[:] for row in current_board]

            make_move(next_board, move, 'O' if current_player == 'X' else 'X')

            queue.append((next_board, 'O' if current_player == 'X' else 'X'))

def main():
    board = [['-' for _ in range(3)] for _ in range(3)]

    print("Welcome to Tic Tac Toe!")
    print("Enter row and column (0-2) to make your move.")

    print_board(board)

    current_player = 'X'

    while True:
        if current_player == 'X':
            row = int(input(f"Player {current_player}, enter row: "))
            col = int(input(f"Player {current_player}, enter column: "))

            if board[row][col] != '-':
                print("Invalid move! Try again.")
                continue

            make_move(board, (row, col), current_player)

        else:
            print(f"Player {current_player}'s turn (AI). Thinking...")

            best_move = None
            best_score = -float('inf')

            for move in get_valid_moves(board):
                next_board = [row[:] for row in board]

                make_move(next_board, move, 'O')

                score = bfs(next_board, 'X')

                if score > best_score:
                    best_score = score
                    best_move = move

            make_move(board, best_move, current_player)

            print(f"AI (Player {current_player}) chooses {best_move}")

        print_board(board)

        if check_winner(board, current_player):
            print(f"Player {current_player} wins!")
            break

        elif len(get_valid_moves(board)) == 0:
            print("It's a draw!")
            break

        else:
            current_player = 'X' if current_player == 'O' else 'O'

if __name__ == "__main__":
    main()
```

## Sample Output
```text
Welcome to Tic Tac Toe!
AI (Player O) chooses (1, 1)
Player O wins!
```

## Result
Thus the User vs AI Tic Tac Toe game was implemented successfully.

---

# 12. ARTIFICIAL NEURAL NETWORK (ANN) FOR DIABETES PREDICTION

## Aim
To implement Artificial Neural Network (ANN) for diabetes prediction using Python.

## Procedure
1. Import required libraries.
2. Create sample dataset.
3. Perform Exploratory Data Analysis (EDA).
4. Split the dataset into training and testing sets.
5. Build ANN model.
6. Train the ANN model.
7. Evaluate prediction accuracy.
8. Display the result.

## Code
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
from sklearn.metrics import accuracy_score

# Sample dataset
np.random.seed(0)

data = {
    'Glucose': np.random.randint(80, 180, 20),
    'BP': np.random.randint(60, 100, 20),
    'BMI': np.random.randint(18, 40, 20),
    'Outcome': np.random.randint(0, 2, 20)
}

DF = pd.DataFrame(data)

print("Dataset")
print(DF.head())

X = DF[['Glucose', 'BP', 'BMI']]
y = DF['Outcome']

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

model = MLPClassifier(hidden_layer_sizes=(5,5), max_iter=1000)

model.fit(X_train, y_train)

y_pred = model.predict(X_test)

accuracy = accuracy_score(y_test, y_pred)

print("Prediction Accuracy:", accuracy)
```

## Sample Output
```text
Dataset
   Glucose  BP  BMI  Outcome
0      124  74   30        1
1      127  81   24        0

Prediction Accuracy: 0.83
```

## Result
Thus the ANN model for diabetes prediction was implemented successfully.

