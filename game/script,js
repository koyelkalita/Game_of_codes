const initialBoard = [
    [5, 3, "", "", 7, "", "", "", ""],
    [6, "", "", 1, 9, 5, "", "", ""],
    ["", 9, 8, "", "", "", "", 6, ""],
    [8, "", "", "", 6, "", "", "", 3],
    [4, "", "", 8, "", 3, "", "", 1],
    [7, "", "", "", 2, "", "", "", 6],
    ["", 6, "", "", "", "", 2, 8, ""],
    ["", "", "", 4, 1, 9, "", "", 5],
    ["", "", "", "", 8, "", "", 7, 9]
];

let board = JSON.parse(JSON.stringify(initialBoard));
let history = [];
let redoStack = [];

window.onload = function() {
    drawBoard();
};

function drawBoard() {
    const boardContainer = document.getElementById("sudoku-board");
    boardContainer.innerHTML = "";

    for (let row = 0; row < 9; row++) {
        for (let col = 0; col < 9; col++) {
            const cell = document.createElement("input");
            cell.type = "text";
            cell.maxLength = 1;
            cell.value = board[row][col];
            cell.readOnly = initialBoard[row][col] !== "";
            cell.addEventListener("input", (e) => handleInput(e, row, col));
            boardContainer.appendChild(cell);
        }
    }
}

function handleInput(e, row, col) {
    const value = e.target.value;
    if (isNaN(value) || value < 1 || value > 9) {
        e.target.value = "";
        return;
    }

    history.push({ row, col, prevValue: board[row][col] });
    board[row][col] = parseInt(value);
    redoStack = [];

    if (checkDuplicate(row, col, value)) {
        e.target.classList.add("duplicate");
        displayMessage("Duplicate number detected!", "red");
    } else {
        e.target.classList.remove("duplicate");
    }
}

function checkDuplicate(row, col, value) {
    for (let i = 0; i < 9; i++) {
        if (i !== col && board[row][i] == value) return true;
        if (i !== row && board[i][col] == value) return true;
    }

    const startRow = Math.floor(row / 3) * 3;
    const startCol = Math.floor(col / 3) * 3;
    for (let i = startRow; i < startRow + 3; i++) {
        for (let j = startCol; j < startCol + 3; j++) {
            if ((i !== row || j !== col) && board[i][j] == value) return true;
        }
    }
    return false;
}

function checkSolution() {
    for (let row = 0; row < 9; row++) {
        for (let col = 0; col < 9; col++) {
            const value = board[row][col];
            if (value === "" || checkDuplicate(row, col, value)) {
                displayMessage("The puzzle is incomplete or has errors!", "red");
                return;
            }
        }
    }
    displayMessage("Congratulations! You solved the Sudoku!", "green");
}

function getHint() {
    for (let row = 0; row < 9; row++) {
        for (let col = 0; col < 9; col++) {
            if (board[row][col] === "") {
                displayMessage(`Try placing a number at (${row + 1}, ${col + 1})`, "blue");
                return;
            }
        }
    }
    displayMessage("No hints available", "blue");
}

function undoMove() {
    if (history.length === 0) {
        displayMessage("No moves to undo", "blue");
        return;
    }

    const lastMove = history.pop();
    redoStack.push({ row: lastMove.row, col: lastMove.col, prevValue: board[lastMove.row][lastMove.col] });
    board[lastMove.row][lastMove.col] = lastMove.prevValue;
    drawBoard();
}

function redoMove() {
    if (redoStack.length === 0) {
        displayMessage("No moves to redo", "blue");
        return;
    }

    const lastRedo = redoStack.pop();
    history.push({ row: lastRedo.row, col: lastRedo.col, prevValue: board[lastRedo.row][lastRedo.col] });
    board[lastRedo.row][lastRedo.col] = lastRedo.prevValue;
    drawBoard();
}

function displayMessage(message, color) {
    const messageDiv = document.getElementById("message");
    messageDiv.innerText = message;
    messageDiv.style.color = color;
}
