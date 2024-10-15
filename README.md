This is my personal project, aiming to learn and practice Machine Learing. 
Because Github could not comment directly like Jupyter Notebook's Raw Cell, I will leave the data information here.

# DATA INFORMATION
The data is downloaded from Kaggle: https://www.kaggle.com/datasets/henriqueyamahata/bank-marketing?select=bank-additional-names.txt

## Input variables:
### Bank client data:
   1 - Age (numeric)
   
   2 - Job : type of job (categorical: "admin.","blue-collar","entrepreneur","housemaid","management","retired","self-employed","services","student","technician","unemployed","unknown")
   
   3 - Marital : marital status (categorical: "divorced","married","single","unknown"; note: "divorced" means divorced or widowed)
   
   4 - Education (categorical: "basic.4y","basic.6y","basic.9y","high.school","illiterate","professional.course","university.degree","unknown")
   
   5 - Eefault: has credit in default? (categorical: "no","yes","unknown")
   
   6 - Housing: has housing loan? (categorical: "no","yes","unknown")
   
   7 - Loan: has personal loan? (categorical: "no","yes","unknown")
### Related with the last contact of the current campaign:
   8 - Contact: contact communication type (categorical: "cellular","telephone") 
   
   9 - Month: last contact month of year (categorical: "jan", "feb", "mar", ..., "nov", "dec")
   
  10 - Day_of_week: last contact day of the week (categorical: "mon","tue","wed","thu","fri")
  
  11 - Duration: last contact duration, in seconds (numeric). 
  
  Important note:  this attribute highly affects the output target (e.g., if duration=0 then y="no"). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.
### Other attributes:
  12 - Campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
  
  13 - Pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)
  
  14 - Previous: number of contacts performed before this campaign and for this client (numeric)
  
  15 - Poutcome: outcome of the previous marketing campaign (categorical: "failure","nonexistent","success")
### Output variable (desired target):
  16 - y - has the client subscribed a term deposit? (binary: "yes","no")

# MY CONCLUSION

## Numerical Variables
  1 - Age: Độ tuổi có phân phối lệch trái, median và mean vào khoảng xấp xỉ 40 tuổi. Có những outliers trên 70 tuổi. Có vẻ độ tuổi có sự tương quan nhẹ tới kết quả cuối cùng khi 'yes' có biên độ rộng hơn so với 'no'. Liệu có phải do những người cao tuổi hoặc những người trẻ có xu hướng gửi tiền có kì hạn hay ko?
  
  2 - Balance: Đa số có số dư tài khoản khoảng $1000 và tồn tại những người có số dư tài khoản rất cao, thậm chí trên $100000. Khi ko tính tới những outliers, dường như có sự tương quan giữa tỉ lệ thành công đối với những tài khoản có số dư cao. 
      Những người này có phải những người cao tuổi, đã về hưu và đồng ý gửi số tiền tiết kiệm của bản thân vào tài khoản có kì hạn hay ko?
      
  3 - Duration: Thời gian liên hệ với khách hàng có phân bố lệch trái, chủ yếu nằm trong khoảng dưới 20s, tồn tại nhiều outliers khá cao. Thời gian liên hệ với khách hàng có tương quan khá mạnh với tỉ lệ thành công của khách hàng, cho thấy liên hệ càng lâu thì tỉ lệ càng cao. 
      Tuy nhiên có thể những khách hàng đã có sẵn nhu cầu nên mới có thể trao đổi lâu, còn những người ko có nhu cầu lại từ chối ngay khi nhận thông tin, chỉ có thể khẳng định rằng thời gian liên hệ có tương quan, chưa thể khẳng định có mối quan hệ nhân quả
      
  4 - Campaign: Số cuộc liên lạc với khách hàng trong chiến dịch này có phân bổ lệch trái, chủ yếu vào khoảng dưới 3 cuộc liên lạc. Dường như ko có sự tương quan nào đối với tỉ lệ thành công và số cuộc liên lạc với khách hàng, thậm chí dường như liên lạc càng nhiều thì lại càng thất bại khi tồn tại rất nhiều outlier ở trường hợp từ chối
  
  5 - Previous: Số cuộc liên lạc với khách hàng trước chiến dịch này có phân bổ lệch trái và đa số chỉ khoảng dưới 3 cuộc. Tuy nhiên có vẻ với những khách hàng đã liên lạc, tỉ lệ gửi tiền thành công cao hơn. Cần nghiên cứu thêm đặc điểm của những người đã từng liên lạc và thành công gửi tiền
  
  6 - Correlations: Tỉ lệ thành công có tương quan khá mạnh với thời gian liên lạc với khách hàng. Tuy nhiên như đã nói ở trên, chưa thể khẳng định là do liên lạc càng lâu thì tỉ lệ thành công càng cao, chưa biết là do chính bản thân khách hàng có nhu cầu hay do giao dịch viên thuyết phục được khách hàng. Ngoài ra ko có tương quan nào đáng kể với biến mục tiêu. 
      Hầu như ko có tương quan nào giữa các biến ngoài previous (số cuộc liên lạc trước chiến dịch marketing hiện tại) và pdays (số ngày từ khi liên lạc cuối cùng với khách hàng)

## Categorical Variables
  1 - Target (y): Tỉ lệ từ chối gửi tiền cao hơn khá nhiều so với tỉ lệ gửi tiền thành công, cho thấy dữ liệu đang bị mất cân bằng, có thể ảnh hưởng tới độ chính xác của mô hình => accuracy có thể sẽ ko hiệu quả để đánh giá mô hình, thay vào đó nên sử dụng f1-score
  
  2 - Marital: Số người đã kết hôn chiếm tỉ lệ áp đảo so với những người độc thân hoặc đã ly hôn, tuy nhiên những người độc thân mới là nhóm có tỉ lệ thành công cao nhất, có thể do họ ko có gánh nặng chi phí cao như những người đã có gia đình
  
  3 - Education: Trình độ học vấn hết cấp 2 chiếm tỉ lệ cao so với những nhóm khác, tiếp đó là tới hết cấp 3. Nhóm người gửi tiền cao nhất đó là nhóm có trình độ học vấn cao nhất, dường như trình độ học vấn càng cao thì tỉ lệ gửi tiền thành công càng lớn
  
  4 - Default: Đa số đều chưa có hoặc chưa sử dụng dịch vụ tín dụng. Có thể thấy nhóm người chưa sử dụng dịch vụ tín dụng lại có tỉ lệ gửi tiền cao hơn tương đối nhiều, tuy nhiên do số lượng người đã sử dụng tín dụng thấp hơn hẳn nên chưa thể khẳng định được điều này
  
  5 - Housing:Nhóm người đang vay nợ mua nhà có số lượng lớn hơn có với nhóm người ko, tuy nhiên nhóm người ko vay nợ lại có tỉ lệ thành công cao hơn nhiều. Có thể giải thích rằng khi họ ko có gánh nặng trả nợ, họ sẽ dễ gửi tiền hơn
  
  6 - Loan: Số người vay nợ cá nhân thấp hơn nhiều so với số người ko vay nợ. Tương tự như vay mua nhà, nhóm người ko vay sẽ có tỉ lệ gửi tiền cao hơn khi họ ko có áp lực phải trả nợ
  
  7 - Contact: Nhóm người được liên hệ mạng di động chiếm đa số. Tuy nhiên ko có sự khác biệt về tỉ lệ thành công giữa các nhóm, duy nhất nhóm 'unknown' có tỉ lệ thành công thấp hơn hẳn mặc dù số lượng chiếm khá nhiều. Cần phân loại nhóm này vào các nhóm khác để có thêm thông tin phân tích
  
  8 - Poutcome: Kết quả của chiến dịch marketing trước đó dường như khá mù mờ khi 'unknown' chiếm tỉ lệ áp đảo, điều này dẫn đến việc phân tích bị ảnh hưởng. Ở chiến dịch này nên thực hiện kĩ càng hơn về việc ghi chép dữ liệu.
      Tỉ lệ thành công đặc biệt cao ở nhóm đã gửi tiền thành công vào chiến dịch trước, cho thấy nhóm khách hàng đã gửi tiền luôn muốn gửi tiền thêm và cần tập trung vào nhóm này. Trong khi đó nhóm 'other' dường như đang lưỡng lự khi có tỉ lệ thành công cũng khá tốt, có thể liên lạc lại và thuyết phục với thời gian lâu hơn
      
  9 - Job: Số người lao động chiếm tỉ lệ cao nhất, sau đó là quản lý và kĩ thuật, cho thấy dữ liệu khá đa dạng các đối tượng. Tuy nhiên, trái với suy nghĩ những người đi làm sẽ sở hữu nhiều tiền và có tỉ lệ gửi tiền cao, thì sinh viên và hưu trí mới là hai đối tượng có tỉ lệ gửi tiền cao nhất. 
      Có thể giải thích rằng nhóm hưu trí đã sở hữu nhiều tài sản và hiện tại muốn gửi tiền, còn sinh viên chưa có nhiều nỗi lo và đang có tư duy quản lý tài chính cá nhân tốt nên có tỉ lệ gửi tiền cao
      

## Relationship Analysis
  1 - Age and Balance: Ko có gì đáng ngạc nhiên, nhóm người trên 30 dưới 45 là nhóm đang đông nhất và sở hữu tài sản trung bình lớn nhất. Nhóm người trẻ dưới 30 và người cao tuổi trên 60 chiếm tỉ trọng khá thấp và cũng ko sở hữu nhiều tài sản. Tuy nhiên dù ko sở hữu nhiều tài sản, nhóm người cao tuổi và nhóm người trẻ lại có tỉ lệ gửi tiền thành công rất cao, thậm chí hơn 40%
  
  2 - Jobs and Education: Những công việc như quản lý, admin, kĩ thuật và khởi nghiệp kinh doanh dường như yêu cầu trình độ học vấn cao (cấp 2 trở lên). Những công việc lao động phổ thông có trình độ học vấn trung bình chiếm đa số (cấp 2 trở xuống)
  
  3 - Education, Job with Balance: Dường như ko có sự khác biệt lớn nào giữa số dư tài khoản theo trình độ học vấn hay công việc. Tuy nhiên nếu xét tới các outliers thì có thể thấy với trình độ học vấn càng cao thì càng tồn tại nhiều outlier có số dư cao. 
      Về công việc, quản lý tồn tại những outliers có số dư tài khoản cao nhất, sau đó là tới nhóm hưu trí, thấp nhất là sinh viên. Điều này chứng minh tỉ lệ gửi tiền thành công có thể phụ thuộc vào những yếu tố khác như tư duy hoặc mục tiêu cá nhân, có vẻ ko liên quan tới số dư tài khoản cao hay thấp
      
  4 - Previous Contact: Dường như không có sự khác nhau giữa phương thức liên lạc và kết quả trong chiến dịch marketing trước đó. Có thể thấy mạng di động là phương thức marketing chủ yếu và đang dần dần phát triển hơn. Marketing thông qua điện thoại di động đang dần nhường chỗ cho mạng di động khi trung bình số ngày kể từ lần liên lạc cuối khá cao mặc dù số lượng liên lạc chiếm thiểu số
      Bên cạnh đó thì khoảng thời gian liên lạc ko có sự khác nhau giữa các phương thức
      

## Overview for training models
   Training models: Dữ liệu có 45211 dòng và 16 cột (không bao gồm cột cuối cùng) => Dữ liệu ko có quá nhiều chiều, số lượng dòng cũng nằm ở mức trung bình, có thể sử dụng tree-based models


## Results
   Random Forest Classifier: Với f1-score là 0.82 cho nhãn số 1 và ROC cũng như AUC gần tuyệt đối, RFC có kết quả rất tốt mặc dù cần khá nhiều thời gian chạy. Tuy nhiên recall cho nhãn số 1 lại chỉ ở mức 0.69, cần cân nhắc điều chỉnh và xem xét liệu RFC có đang bị overfit hay không bằng cách thêm dữ liệu hoặc điều chỉnh lại hyperparameters
   Gradient Boosting Classifier: f1-score là 0.72 cho nhãn số 1 và ROC/AUC khá tốt (khoảng 0.96), GBC có kết quả tương đối ổn. Tuy nhiên tốn nhiều thời gian để xử lý nhất. Cũng giống như RFC, recall cho nhãn số 1 cũng khá thấp (khoảng 0.57). Cần tối ưu hơn
   XGBoost Classifier: XGC có f1-score là thấp nhất do precision và recall đều thấp, tuy nhiên lại có thời gian chạy rất nhanh, chỉ bằng 1/2 các models trên. Hiện XGC đang là model có kết quả chưa tốt nhất, cần tối ưu lại hyperparameter vì XGB có tiềm năng khá cao
