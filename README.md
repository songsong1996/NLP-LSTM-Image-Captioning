# NLP-LSTM-Image-Captioning  
Goal: Using LSTM to learn the relationship between images and captioning and generate captions for new images.  
目标: 使用LSTM网络学习图片和其注释, 使得网络可以根据图片生成描述注释.  
## Data Source  
flickr8k dataset  
引用于 M. Hodosh, P. Young and J. Hockenmaier (2013) "Framing Image Description as a Ranking Task: Data, Models and Evaluation Metrics", Journal of Artificial Intelligence Research, Volume 47, pages 853-899 http://www.jair.org/papers/paper3994.html when discussing our results  
## Main Steps  
* Create matrices of image representations using an off-the-shelf image encoder.  
生成
* Read and preprocess the image captions.   
* Write a generator function that returns one training instance (input/output sequence pair) at a time.   
* Train an LSTM language generator on the caption data.  
* Write a decoder function for the language generator.  
* Add the image input to write an LSTM caption generator.  
* Implement beam search for the image caption generator.  
## Image Encoder 图片编译  
- Single Image Encoder 单张图片编译  
使用off-the-shelf pre-trained image encoder, the Inception V3 network.  
该网络是一个物体检测CNN 网络. 细节见下文: The model is a version of a convolution neural network for object detection. Here is more detail about this model (not required for this project):  
Szegedy, C., Vanhoucke, V., Ioffe, S., Shlens, J., & Wojna, Z. (2016). Rethinking the inception architecture for computer vision. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 2818-2826). https://www.cvfoundation.org/openaccess/content_cvpr_2016/html/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.html  
- 生成iterator迭代器  
使用yield语法, 生成图片的generator, 方便后续训练  
## Text (Caption) Data Preparation 文字数据处理  
- Tokenization 分割text  
- Build Dictionary 新建词典  
word_to_id:dict for encoding, id_to_word:dict for decoding  
## Basic Decoder Model 基本模型  
核心思想是Keras循环层（包括LSTM）创建一个“展开的” RNN。 每个时间步表示为一个不同的单位，但是这些单位的权重是共享的。 我们将使用常量MAX_LEN来引用序列的最大长度，该序列的最大长度为40个字（包括START和END)  
- Input 输入数据  
在课堂上，我们讨论了LSTM生成器作为转换器，将输入序列中的每个单词映射到下一个单词    
此处，我们将使用模型在给定部分序列的情况下一次预测一个单词。 例如，给定序列[“ START”，“ a”]，模型可以预测“ dog”是最可能的单词  
## Training Model 模型训练  
accuracy=42%   
['<START>', 'a', 'man', 'in', 'a', 'black', 'and', 'white', 'suit', 'is', 'in', 'front', 'of', 'a', 'man', 'with', 'dog', 'in', 'a', 'blue', 'shirt', '<END>']  
['<START>', 'a', 'a', 'white', 'dog', 'walking', 'in', 'a', 'grassy', 'field', 'with', 'a', 'stick', 'in', 'his', 'mouth', '<END>']  
## Conditioning on Image 图像调节  
对Model进行修改,使其可以学习图片image信息  
accuracy=41%  
## Test 结果验证   


