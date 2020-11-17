# WU CTF CHO CÁC THÀNH VIÊN TRAINING KHÔNG ĐI QUÂN SỰ
# Sudoku  
## Source code:
```
int main()
{
	string flag1="ispclub{d0_y0u_l1k3_5ud0ku?}";
	string flag2="ispclub{l3t'5_try_4_puzzl3!}";
	string flag3="ispclub{y0u_d0_n0t_g3t_th3_fl4g}";
	string flag4="abcdefg{xxxxxxxxxxxxxxxxxxxxxxxxx}";
	string sudoku="haagaacadacaibaafaaafajcaaacaaahafaidahaajacaajaaeaaahbaiadhaacaaabaaiaagacaaajda";
	
	TextColor(2);
	cout << flag1 << endl;
	cout << "(Y or N)? ====> Answer: ";
	char answer;
	cin >> answer;
	cout << endl;
	
	if (answer=='Y' || answer=='y'){
		cout << flag2 << endl << endl;
		TextColor(4);
		int c=-1;
		for (int i=0; i<13; i++){
			for (int j=0; j<13; j++){
				if (i%4==0) cout << "--";
				else if (j%4==0) cout << "| ";
				else cout << sudoku[++c]-97 << " ";
			}
			cout << endl;
		}
		TextColor(14);
		flag4=flag1.substr(0, sudoku[2]-sudoku[13]) + flag1.substr(20, sudoku[72]-97) +  flag2.substr(19, sudoku[38]-97) + "_";
		flag4.push_back(sudoku[32]-49);
		flag4.push_back(sudoku[33]-49);
		flag4 += flag3.substr(sudoku[2]+sudoku[4]-97*2, sudoku[2]-sudoku[5]) + "345y}"; 
		
		if (flag4.size()==34) cout << flag4 << endl;
		else cout << "Try again";
	}
	else{
		TextColor(12);
		cout << flag3 << endl;
	}
	return 0;
}
```
Đây là một bài code C++. Đọc code ta thấy có 3 flag đúng form “ispclub{this_is_flag}” và 1 "abcdefg{xxxxxxxxxxxxxxxxxxxxxxxxx}", mà chiều dài xâu “abcdefg” lại bằng đúng chiều dài “ispclub”. Vậy liệu đây có phải flag thật không? Ta chạy thử code xem ra gì nhé.  
```
	ispclub{d0_y0u_l1k3_5ud0ku?} 			|		ispclub{d0_y0u_l1k3_5ud0ku?}
	(Y or N)? ====> Answer: Y			|		(Y or N)? ====> Answer: N
							|
	ispclub{l3t'5_try_4_puzzl3!}			|		ispclub{y0u_d0_n0t_g3t_th3_fl4g}
							|
	--------------------------			|
	| 7 0 0 | 6 0 0 | 2 0 3 |			|
	| 0 2 0 | 8 1 0 | 0 5 0 |			|
	| 0 0 5 | 0 9 2 | 0 0 0 |			|
	--------------------------			|
	| 2 0 0 | 0 7 0 | 5 0 8 |			|
	| 3 0 7 | 0 0 9 | 0 2 0 |			|
	| 0 9 0 | 0 4 0 | 0 0 7 |			|
	--------------------------			|
	| 1 0 8 | 0 3 7 | 0 0 2 |			|
	| 0 0 0 | 1 0 0 | 8 0 0 |			|
	| 6 0 2 | 0 0 0 | 9 3 0 |			|
	--------------------------			|
	Try again					|
```
Ta thấy 3 flag đầu là flag giả. ta thấy 1 ma trận 9x9 số, quay về đọc code xem ta có gì:  

- Dòng 20: *string sudoku*  có 81 ký tự = 9x9. Vậy đây chắc chắn là ma trận sudoku rồi. Nhưng tại sao lại là chữ cái? 
- Dòng 37: *sudoku[++c]-97* dùng để chuyển các ký tự từ chữ về số theo mã ASCII.
- Dòng 42 - 45: Ta thấy string flag4 phụ thuộc vào các string flag1, flag2, flag3, sudoku. 
- Dòng 47: *if (flag4.size()==34) cout << flag4 << endl;*  nếu size flag4 bằng đúng với 34 ( = với size của flag4 dòng 19) thì sẽ in ra flag4. Vậy ta kết luận flag4 là flag của bài.  

Bây giờ ta phải đi giải bài sudoku này, thay các giá trị 0 bằng những số thích hợp. Ta chuyển dãy sudoku ấy về xâu chữ cái với *‘0’ = ’a’, ’1’ = ’b’, ’2’ = ’c’,...* Mình dùng 1 script python nhỏ để làm việc này:  
```
sudoku="haagaacadacaibaafaaafajcaaacaaahafaidahaajacaajaaeaaahbaiadhaacaaabaaiaagacaaajda"
for i in sudoku:
	print(ord(i)-97,end='')
```
Sau khi chạy ta nhận được output:
`700600203020810050005092000200070508307009020090040007108037002000100800602000930`  

Sau đó các bạn có thể tìm 1 tool hoặc giải tay để luyện thêm trí não, còn ở đây mình tìm thấy 1 tool giúp giải sudoku khá tiện và mình đã chỉnh sửa lại để phù hợp giải chall này:  
```
	s = []
	text =""
	# chuoi string input vao
	sudoku="haagaacadacaibaafaaafajcaaacaaahafaidahaajacaajaaeaaahbaiadhaacaaabaaiaagacaaajda"
	
	# chuyen tu chuoi ky tu thanh chuoi so
	for i in sudoku:
	text += str(ord(i)-97)
	
	# chuyen chuoi so thanh dang ma tran 9*9 va giai sudoku
	while len(text) > 0:
	l = []
	while len(l) < 9:
	    if text[0].isdigit():
		l.append(int(text[0]))
	    text = text[1:]
	s.append(l)
	if len(s) == 9:
	    initial_try(s)
	    for line in s:
		if 0 in line:
		    DFS_solve(s, 0, 0)
		    break
	# in ra string solved theo format cua challenge
	for row in range(9):
	for col in range(9):
	    print(chr(int(s[row][col])+97), end='')
```
[source code](https://pastebin.com/vmYLSBNP)  

Sau khi chạy tool mình nhận được string sudoku solved: `hbjgfecidecgibdhfjidfhjcebgcgedhbfjidihfgjbcefjbceidghbfijdhgecjedbcgihfghceifjdb`  

Sau đó thay chuỗi *sudoku="hbjgfecidecgibdhfjidfhjcebgcgedhbfjidihfgjbcefjbceidghbfijdhgecjedbcgihfghceifjdb"* vào source code challenge ban đầu và chạy lại:  
```
	ispclub{d0_y0u_l1k3_5ud0ku?}
	(Y or N)? ====> Answer: Y

	ispclub{l3t'5_try_4_puzzl3!}

	--------------------------
	| 7 1 9 | 6 5 4 | 2 8 3 |
	| 4 2 6 | 8 1 3 | 7 5 9 |
	| 8 3 5 | 7 9 2 | 4 1 6 |
	--------------------------
	| 2 6 4 | 3 7 1 | 5 9 8 |
	| 3 8 7 | 5 6 9 | 1 2 4 |
	| 5 9 1 | 2 4 8 | 3 6 7 |
	--------------------------
	| 1 5 8 | 9 3 7 | 6 4 2 |
	| 9 4 3 | 1 2 6 | 8 7 5 |
	| 6 7 2 | 4 8 5 | 9 3 1 |
	--------------------------
	ispclub{5ud0ku_puzzl3_15_n0t_345y}
```
Flag: `ispclub{5ud0ku_puzzl3_15_n0t_345y}`
