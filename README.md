# Trie

[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn về Trie và các vấn đề ví dụ
Nội dung trong bài viết này là về Tries và các khái niệm được sử dụng rộng rãi trong vài ứng dụng vào các vấn đề cụ thể. Chúng ta sẽ xem xét 2-3 vấn đề mà trie sẽ hữu ích.

Đầu tiên chúng ta sẽ xem trie là gì. Trie có thể lưu trữ thông tin về các khóa/số/chuỗi 1 cách nhỏ gọn trong 1 cây. Các tries bao gồm các node, mỗi node lưu trữ 1 kí tự/bit. Chúng ta có thể thêm các chuỗi/số mới phù hợp.

Đây là một ví dụ trie về chuỗi:


![][1]

  
Nguồn: Wikipedia.

Nhưng giờ chúng ta sẽ giải quyết với các số, đặc biệt là các số ở dạng các bit nhị phân. Chúng ta sẽ hiểu rõ hơn khi giải quyết các vấn đề.

**Vấn đề 1**:   Cho 1 mảng các số nguyên, chúng ta phải tìm ra 2 phần tử có XOR lớn nhất.
**Giải quyết:**  
Giả sử chúng ta có 1 cấu trúc dữ liệu có thể thỏa mãn 2 loại truy vấn sau:
1\. Thêm 1 số X  
2\. Cho 1 số Y, tìm XOR tối đa của Y với tất cả các số đã được thêm vào cho tới hiện tại.


Nếu chúng ta có được cấu trúc dữ liệu này, chúng ta sẽ dễ dàng thêm các số nguyên vào, và với truy vấn loại 2 chúng ta sẽ tìm được XOR tối đa.
Trie là cấu trúc dữ liệu chúng ta sẽ sử dụng. Đầu tiên hãy xem cách chúng ta thêm các phần tử vào trie.

![][2]

  
Vì vậy, chúng ta sẽ lần theo các đường đi của các số mà chúng ta đã thêm vào, chúng ta sẽ không phải vẽ lại các đường các tồn tại nữa.

  Việc thêm khóa có độ dài N mất O(N), là log2(MAX) trong đó MAX là số tối đa được thêm vào trong trie, bởi vì có tối đã log2(MAX) bit nhị phân trong 1 số.
Bằng cách này, chúng ta lưu tất cả dữ liệu về các số được thêm vào trong trie cho đến bây giờ.

Giờ với  câu truy vấn loại 2:
Giả sử số Y của chúng ta là b1,b2..bn, trong đó b1,b2.. là các bit nhị phân. Chúng ta sẽ bắt đầu từ b1. Giờ để cho XOR đặt giá trị lớn nhất, chúng ta sẽ cố gắng biến most significant bit là 1 sau khi thực hiện XOR. Vì vạy, nếu b1 là 0, chúng ta sẽ cần 1 và  ngược lại. Trong trie, chúng ta sẽ đi về các phía có các bit yêu cầu. Nếu  không có lựa chọn tối ưu, chúng ta sẽ đi về phía khác. Thực hiện điều này cho tất cả từ i=1 cho tới n, chúng ta sẽ lấy được giá trị lớn nhất của XOR .

![][3]

Query too is log2(MAX).

**Vấn đề 2**: Cho 1 mảng số nguyễn, tìm mảng con có XOR tối đa.
**Giải phép:**  
Gọi F(L,R) và XOR của mảng con từ L tới R.
Chúng ta sẽ sử dụng tính chất F(L,R) = F(1,R) XOR F(1,L-1). Bằng cách nào?
Giả sử mảng con của chúng ta với XOR tối đa kết thúc tại ví trí i. Giờ chúng ta sẽ cần tối đã F(L,i) ví dụ F(1,i) XOR F(1,L-1) trong đó L <= i. Giả sử chúng ta đã thêm F(1,L-1) vào trie của chúng ta với tất cả L<=i, giờ nó trở thành vấn đề 1.

    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

chúng ta có thể thử vấn đề này ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Cho 1 mảng các số nguyên dương, bạn phải in số của các mảng con mà XOR của nó nhỏ hơn K.


**Giải pháp:** 
Vấn đề này 1 lần nữa sử dụng các khái niệm mà chúng ta đã thấy cho tới giờ. Chúng ta sẽ thực hiện như với câu hỏi trước.
Với mỗi chỉ mục i=1 đến N, chúng ta có thể đếm có bao nhiêu mảng con kết thúc tại vị trí thứ i thỏa mãn điều kiện đưa ra.
 

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  

query(q,k) trả về kết quả có bao nhiêu số nguyên đã tồn tại trong cấu trúc mà khi thực hiện xor với q trả về 1 số nguyên nhỏ hơn k.
Chúng ta số sánh các bit tương ứng của q và k, bắt đầu từ các most significant bit. Giả sử p và q lần lượt là các bit chúng ta đang xem xét.
Nếu q là 1, và p là 0, chúng ta sẽ làm thế này:  

![][5]

Tương tự chúng ta có thể dễ dàng làm với 3 trường hợp khác như (q=1,p=1), (q=0,p=1) vaf (q=0,p=1).


Vì vậy, chúng ta sẽ cần thay đổi cấu trúc của chúng ta, chúng ta cũng giữ các số của các lá từ node hiện tại nếu chúng ta đi về phía bên trái và tương tự với phía bên phải. Bởi vì, nếu không làm thế, sẽ khiến bài toàn càng trở nên phức tạp hơn, nếu chúng ta duyệt đi duyệt lại toàn bộ cây. Chúng ta có thể làm điều này trong khi thêm các số vào cây 1 cách dễ dàng.

Vấn đề này có trong CodeCraft'14. Bạn có thể thực hành tại đây: [SPOJ.com - Problem SUBXOR][6]

Giờ, hãy bàn về các triển khai.
Về triển khai của 1 trie trong C/CPP là chúng ta có thể giữ các node và các con trỏ trái và phải. Chúng ta có thể viết các hàm đệ quy.

    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
  Với các truy vấn, chúng ta cũng có thể duyệt cây 1 cách đệ quy.


Cập nhật:  
Các vấn đề khác khii sử dụng Trie(yay! :P).  
[Problem - B - Codeforces][7]

**Vấn đề phụ**: Cho 1 tập gồm n chuỗi không rỗng. Trong suốt trò chơi, 2 người chơi sẽ  cùng nhau xây dựng các từ, bắt đầu với các từ rỗng. Các người chơi sẽ thực hiện theo lượt. Vào lượt của mình, người chơi phải thêm 1 kí tự đơn vào cuối từ, kết quả của từ phải là tiền tố của ít nhất 1 chuỗi trong nhóm. 1 người chơi sẽ thua nếu không thể tiếp tục.

Chúng ta cần tìm ra người chơi nào (thứ nhất hay thứ hai) có khả năng chiến thằng.


Vậy ý  tưởng ở đây là lại xây dựng 1 trie gồm tất cả các chuỗi. Tại sao? Bởi vì 1 trie sẽ lưu tất cả các thông tin về tất cả các tiền tố.
Giờ chúng ta sẽ cố đánh giá từng node để xem người chơi đầu tiền có khả năng chiến thẳng hay không. Chúng ta có thể thực hiện điều này bằng đệ quy. Với 1 node v, với mỗi node u mà u là con trực tiếp của v, nếu người chơi đầu tiên có khả năng thua vì u, thì với node v người chơi đầu tiên có khả năng chiến thắng. 
Ví dụ, chúng ta có "abc", "abd", "acd".
Trie của chúng ta sẽ trông như này:

![][8]

  
Tất cả các node lá sẽ đều có khả năng chiến thắng..

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c
