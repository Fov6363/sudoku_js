# sudoku_js JS求解数独,暂不适合需要猜测数独
```
let _arr = [
    [1,0,0,0,0,0,7,0,0],
    [0,2,8,0,1,0,0,5,0],
    [6,3,0,4,0,0,9,0,2],
    [9,6,0,0,3,4,0,8,7],
    [0,0,0,2,9,0,0,6,0],
    [7,0,0,8,5,0,3,9,1],
    [2,0,4,5,0,8,1,3,6],
    [8,7,3,0,0,1,5,2,4],
    [5,1,6,0,0,0,0,7,9],
];

let arr = [
    [1,0,0,0,0,0,7,0,0],
    [0,2,8,0,1,0,0,5,0],
    [6,3,0,4,0,0,9,0,2],
    [9,6,0,0,3,4,0,8,7],
    [0,0,0,2,9,0,0,6,0],
    [7,0,0,8,5,0,3,9,1],
    [2,0,4,5,0,8,1,3,6],
    [8,7,3,0,0,1,5,2,4],
    [5,1,6,0,0,0,0,7,9],
];

let full = [1,2,3,4,5,6,7,8,9];
function handle(arr) {
    for(let i = 0;i < 9;i++){
        for(let j = 0;j < 9;j++){
            if(arr[i][j] === 0){
                //空点,尝试可能的值

                //先找横
                let row = arr[i];
                const row_val = get_can_val(row,j);
                if(row_val.length === 1){
                    console.log('row_val = ' + row_val[0]  + `, i = ${i} ,j = ${j}`);
                    arr[i][j] = row_val[0];
                    continue;
                }

                //再找竖
                let col = get_col(arr,j);
                const col_val = get_can_val(col,i);
                if(col_val.length  === 1){
                    console.log('col_val = ' + col_val[0]  + `, i = ${i} ,j = ${j}`);
                    arr[i][j] = col_val[0];
                    continue;
                }

                //再找九宫格
                let nine = get_nine_val(arr,i,j);
              
                const nine_val = get_right_join(full,nine);
                if(nine_val.length === 1){
                    console.log('nine_val = ' + nine_val[0]  + `, i = ${i} ,j = ${j}`);
                    arr[i][j] = nine_val[0];
                    continue;
                }

                //做一个聚会
                let join = get_union_join(row_val,col_val,nine_val);
                if(join.length === 1){
                    console.log('join = ' + join[0]  + `, i = ${i} ,j = ${j}`);
                    arr[i][j] = join[0];
                    continue;
                }

            }
        }
    }
}

function get_nine_val (arr,i,j){
    let start_i = 0;
    let end_i = 0;
    let start_j = 0;
    let end_j = 0;

    let arr_1 = [];
    if(i <= 2){
        //上三层
        start_i = 0;
        end_i = 2;
    }else if(i <= 5){
        //中三层
        start_i = 3;
        end_i = 5;
    }else{
        //下三层
        start_i = 6;
        end_i = 8;
    }

    if(j <=2){
        //左
        start_j = 0;
        end_j = 2;
    }else if(j <=5){
        //中
        start_j = 3;
        end_j = 5;
    }else{
        //右
        start_j = 6;
        end_j = 8;
    }

    for(let t = start_i;t <= end_i;t++){
        for(let k = start_j;k <= end_j;k++){
            if(t == i && k == j){

            }else{
                arr_1.push(arr[t][k]);
            }
        }
    }

    return arr_1;
}

function get_col(arr,j){
    let arr_1 = [];
    for(let i = 0;i < arr.length;i++){
        arr_1.push(arr[i][j]);
    }

    return arr_1;
}

function get_can_val(arr,j){
    let arr_1 = [];
    for(let i = 0; i < arr.length;i++){
        if(i === j){
        }else{
            arr_1.push(arr[i]);
        }
    }

    return get_right_join(full,arr_1);
}

function get_right_join(arr,arr_1){
    return arr.filter(item => {
        return !arr_1.includes(item);
    })
}

function compare(arr1,arr2){
    for(let i = 0;i < arr1.length;i++){
        for(let j = 0;j < arr1[i].length;j++){
            if(arr1[i][j] !== arr2[i][j]){
                console.log(`i = ${i} ,j = ${j}`);
            }
        }
    }
}

function check_empty(arr){
    for(let i = 0;i < arr.length;i++){
        for(let j = 0;j < arr[i].length;j++){
            if(arr[i][j] === 0){
                return true;
            }
        }
    }

    return false;
}

function get_union_join (){
    let arr = arguments[0];
    for(let i = 1;i < arguments.length;i++){
        arr = arr.filter(item => {
            return arguments[i].includes(item);
        })
    }

    return arr;
}

function core(){
    let count = 0;
    while(check_empty(arr)){
        handle(arr);
        count++;
        if(count === 80){
            break;
        }
    }
    
    compare(_arr,arr);
    console.log(arr);
}

core();
```
