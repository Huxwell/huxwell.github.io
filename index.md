# Programming tips blog 
A template for simple python scripts:

```
""" 
Docstring: I hate when ex-java developers spam unnecessary boilerplate abstractions that do nothing 'just-in-case' so that the code is 'future-proof' and 'universal'.
Nevertheless, in each ML project I find myself surrounded by dozens of throwaway scripts that get really messy when shared over github/box/gdrive/devboxes/ec2 instances.
Please use it as your default starting python file.
"""
__author__ = "Filip Drapejkowski"
__version__ = "1.0.0"

import argparse

def main(args):
    print("Reading from ", args.input_path)
if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="This script does nothing.")
    parser.add_argument("--input_path", help="Input txt file path with data about nothing.", default="input_file.txt")
    parser.add_argument("--output_path", help="Output file that can be loaded into oblivion.", default="output_file.json")
    parser.add_argument('--verbose', '-v', action='count', default=0)
    parser.add_argument("--version", action="version", version="%(prog)s " + __version__)
    args = parser.parse_args()
    main(args)
```

12 Nov 2021
----

A few rarely seen functions and python features that come in handy while debugging.
`global()` and `local()` are super useful when weird things happen in your code and it's due to variable scopes:


```
print(locals() is globals()) #True
def foo():
	a=12; b=13; c=[1,"2",["three"]]
	print(locals() is globals(), locals())
foo() # False, a dictionary with local variables and their values

print(globals)
```
Run these to understand better pythons's modules or for debugging when multiple versions of the same library (or multiple version of Python) might be in conflict:

```
import sys
import math
# print(sys.modules) # lots of output
print(sys.path) # shows the paths that are used for importing modules!
print(math.__dict__)
import fractions
print(sys.modules['math'])
print(sys.modules['fractions'])
print(dir(fractions)) # dir(module) may just return module.__dict__ in some situations
print(fractions.__dict__)
print(fractions.__file__)
```

11 Nov 2021
----

I've been using python 'is' keyword without reflecting what I am actually doing, so here comes a snippet that explains it really well imho:
```
# The 'is' keyword is used to test if two variables refer to the same object.
x = 3; y = 3; z = x
print(x is y, x is z, id(x), id(z), id(y)) # True, True, 3x the same id; nothing suprising for numeric literals
x = [1,2,3]; y = [1,2,3] ; z = x
print(x is y, x is z, id(x), id(z), id(y)) # False, True, 2x the same id; different objects despite the same content
```

9 Nov 2021
----

Some short snippets that come in handy when I need to explain some more obscure features of Python:

Lambdas examples:
```
print((lambda x, y, third_arg, blablabla: x * y - third_arg + blablabla)(5,100,501,2))

list_1 = [1,2,3,4,5,6,7,8,9]
print(list(filter(lambda x: x%2==0, list_1)))
print(list(map(lambda x: pow(x,3), list_1)))

```
Decorators (with arguments):
```
def my_decorator(a_function_that_will_be_wrapped):
    def a_wrapper_function(*args, **kwargs):
        print("="*20)
        a_function_that_will_be_wrapped(*args, **kwargs)
        print("="*20)
    return a_wrapper_function

@my_decorator
@my_decorator
@my_decorator
def print_something_twice(the_text):
    print(the_text)
    print(the_text)

print_something_twice("Hello World!")  
```

4 Nov 2021
----

While installing ROS2 and Carla simulator on Ubuntu I got the following error:
W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list.d/ros2-latest.list:1 and /etc/apt/sources.list.d/ros2.list:1

The reason was running from one guide:
```
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
```
And from another guide writing some lines to /etc/apt/sources.list.d/ros2.list .

All the stackoverflow/ros boards suggestions regarding editing /etc/apt/sources.list didn't help, but the solution I figured was simple:
etc/apt/sources.list.d is a directory, so I just deleted all files from there and ran "sudo apt update". 

5 Jul 2021
----
How to deal with remote ssh in vscode (visual studio code) malfunctioning, hanging etc:

ctrl + shift + p and enter
Remote-SSH: kill VS Code Server on Host...

Than wait patiently for few minutes (maybe close vscode if it hangs; maybe kill it from htop if needed / if there is no reposnde for ctrl + shift + p).

25 Jun 2021
----

Changing the subject: the easiest way I know to check image size (and other info from bash):
file /path/to/image.png

Btw, Sorry for lack of updates, but I'm recently spending a lot of free time on developing an Android app.
The app is supposed to help in practicing math/deep learning facts/computer vision tricks and is going to be called 'brainramen'.

25 Jun 2021
----

Cheatsheet for running multiple programs in one line in bash:
```
A; B    # Run A and then B, regardless of success of A
A && B  # Run B if and only if A succeeded
A || B  # Run B if and only if A failed
A &     # Run A in background.
```
which explains why my docker build [...] docker run was running the old image (not the rebuilt one)

17 May 2021
----

I've made up a nice quiz question about gradient backpropagation, give it a try:

Automatic differentiation is...

a) ...also called numerical differentiation. It uses 2 arguments in the small vincinity of the argument for which we want to compute the derivative. 
It computes function values, divides difference in values by difference in arguments and the resulting slope is an estimate of the derivative we were looking for. 

b) ...also called symbolic differentiation. It exploits the chain rule and the fact that complex functions consist of small subproblems 
which are easy to differentiate. The resulting formula is simplified to a short and elegant form.

c) ...also called computational differentiation. It exploits the chain rule and the fact that complex functions consist of small subproblems 
which are easy to differentiate. Lack of simplification of the final formula allows optimizations i.e. to quickly compute and reuse partial derivatives 
with respect to many inputs.

d) All of above.



Ok, the correct answer is AFAIK (I cannot just write the letter, cause you will read it before thinking for yourself):
Not the first letter of alphabet, not the second, and not the fourth.
Why would you care? Well PyTorch uses automatic differentiation, and if I understand correctly caffe does something in between symbolic and computational.
You have to derive derivatives for backward pass for each layer symbolically but the framework will take care of chain rule for different layers.
Still need to check what other frameworks do exactly.

Update: TensorFlow also does automatic differentiation with "gradiet tape", ref:
https://jonathan-hui.medium.com/tensorflow-automatic-differentiation-autodiff-1a70763285cb
https://www.tensorflow.org/guide/autodiff

4 May 2021
----

diff from bash is actually nice if you ignore whitespaces (! for jupyter):
!diff -w some_predictions.txt some_other_predictions_that_should_be_the_same_but_who_knows.txt

Btw the problem from 10 Apr 2021 is kinda solved.
My code based on pytorch-YOLOv4 was giving wrong activations (confidence scores).
https://github.com/Tianxiaomo/pytorch-YOLOv4 actually gives almost perfect activations.
https://github.com/hunglc007/tensorflow-yolov4-tflite will give me wrong activations.
Something might be off with NMS, but that I am still investigating.
The problem is that pytorch-YOLOv4 uses IoU threshold while Darknet has hidden hier_thresh and nms_thresh.
Will try pytorch-YOLOv4 with 0.0 IoU threshold and see what happens.
Update: IoU threshold 0.45-0.5 in pytorch-YOLOv4 seems to be the best choice for fidelity. The scores won't be identical, but close enough
(in fact mAP slightly better with PyTorch than in original Darknet model).

23 Apr 2021
----

Just noticed the "File->Download as->PDF via LaTeX" functionality in jupyter. It requires
!sudo apt-get install texlive-xetex texlive-fonts-recommended texlive-generic-recommended
...and maybe clearing-up too-long outputs, because of lack of embedded windows with scrolling in pdfs.

But it's often quite nice for a) comparing detailed metrics of trained ML models b) debugging differences in pre- and postprocessing
c) Jira and documentation duties.

23 Apr 2021
----

I've recently encountered a fascinating problem: yolov4 models perform worse after conversion from darknet to TFLite, ONNX or PyTorch.
It's fascinating, that the repos are so popular, but nobody have noticed / solved it yet. Used following repos:

https://github.com/Tianxiaomo/pytorch-YOLOv4
https://github.com/hunglc007/tensorflow-yolov4-tflite

They are not fully blind, but activation values ('probabilities') differ, and some detections are missing.
First, I thought Non-Maxiumum Supression is the reason (it has multiple implementations), then I figured maybe something is different with preprocessing.
So I made sure normalization is the same, and I've learned yolov4 on square 416x416 images (instead of horizontal rectangles) to reduce potential for error.
But It didn't help.

I think I've solved a major bug (dangling skip-links):
https://github.com/Tianxiaomo/pytorch-YOLOv4/issues/370
submitted a PR:
https://github.com/Tianxiaomo/pytorch-YOLOv4/pull/412
but the discrepancy in scores remains.

Btw, here are some examples of why I am hesitant to calling activation function values (from softmax or sigmoid) probabilities or confidence scores:
https://stats.stackexchange.com/questions/309642/why-is-softmax-output-not-a-good-uncertainty-measure-for-deep-learning-models

10 Apr 2021
----
I used to "print to pdf" using system dialog to get rid of additional pages of documents easily. 
Apparently the pdf becomes a picture and you can't copy text from it anymore. Switched to:
pdftk ~/Desktop/too-much-pages.pdf cat 3-7 output ~/Desktop/less_pages.pdf

9 Apr 2021
----
Running darknet binary for yolo predictions from jupyter notebook (or colab, or python in general) without mess in output:

```
import subprocess
command = " ".join(['./darknet detector test',test_data,cfg_path,best_weights,im_path,'-thresh 0.15'])
process = subprocess.Popen(command.split(), stdout=subprocess.PIPE)
output, error = process.communicate()
print(output.decode())
# Works nicely with image.c modification (1 Apr 2021 post)
```

Resize images in dir in bash:
```
sudo apt-get install imagemagick
convert *.png -verbose -resize 640x480 my_dir/*.png
```
Or even better, so that we keep the same filenames:
```
cp -rv my_dir my_dir_backup
mogrify *.png -verbose -resize 640x480 my_dir/*.png
```
Verbose, because sometimes it looks like it failed, while everything was ok.

7 Apr 2021
----
I use bash (not zsh) and I am not a big fan of fancy plugins or complicated, customized setups.
With that being said, I still use guake and fzf. The former provides fuzzy search in history and better autocompletion, and is surprisingly easy to setup.
It uses Levenstein or edit distnace to find the closest command to the thing you've typed in your history or files structure.
I put it in every AWS instance / devbox / laptop that I use.

Currently I setup it with:
```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```
Agree to everything.
```
source ~/.bashrc 
```
So I don't have to reboot/relogin.
Than ctrl + r is enough to start fuzzy searching the history.

6 Apr 2021
----
If you run detection with darknet (i.e yolov3 or yolov4), it's nice to know activation function values (aka. confidence level, although it's not very precise).
For each detection darknet may print it, but it's often hard to match the values with the actual bounding boxes.

You can modify src/image.c file and rerun make all in darknet directory to draw these values on top of the bounding boxes.
Just add:

```
const int best_class = selected_detections[i].best_class;
char prop_str[256];
strcat(labelstr, names[selected_detections[i].best_class]);
sprintf(prop_str, " \n%f", selected_detections[i].det.prob[best_class]);
strcat(labelstr, prop_str);
```

After line 444, although it might change over time.
It should be in ```draw_detections_v3()``` even if you use yolov4; has to appear before get_label_v3() call but after labelstr is initiated.

1 Apr 2021
----
Piping in shell/bash sometimes works with simple |, sometimes not (depending on how arguments are parsed by given command/program).
But simply adding 'xargs' seems to solve most of the problems:

To check if paths in list of files look ok (if first file exists):
```
!head -1 images_list.txt | xargs du
```
Sed in pipe to check if annotation file for darknet (named as image, only with .txt extension) exists / what it contains:
```
!head -1 devel_images_list.txt | sed 's/.png/.txt/' | xargs cat
```
(Don't ask me why no 'xargs' in sed; for now I am just pasting 'xargs' all over the place and see whether it works or not).

25 Mar 2021
----
The -u option in cp often help when you are to lazy to use rsync, but copying takes too long and also was interupted, here is what it does:
'copy only when the SOURCE file is newer than the destination file or when the destination file is missing'
I often endup using:
```
cp -vru old_dir/ new_dir/
```
Checking the size and other info about an image in shell:
identify color.jpg
Will require some version of imagemagick, I just did 'apt-get install' the first proposed one.

PIL->OpenCV conversion:
```
from PIL import Image
img = PIL.Image.open('img.jpg').convert('RGB') 
img = numpy.array(img) 
img = img[:, :, ::-1].copy() #RGB to BGR 
```
Width height order:
```
height, width = image.shape[:2] #cv2
width, height = image.size[:2] #PIL
```
Reading width and height without loading the whole image to memory - just open with PIL and don't display/modify:
```
image = Image.open(path)
```
19 Mar 2021
----
Replace first char in each line inplace in all .txt files in directory:
```
sed -i 's/^./new_string/g' *.txt
```
Use carefully! I've damaged some datasets by mistake with it.

Delete all empty txt files:
```
find . -name '*.txt' -size 0 -delete
```
15 Mar 2021
----
Less with line numbering (-N) and starting from line (+n):
```
less +29300 -N my_very_long_file_with_stupid_newline_missing_in_the_middle_of_it.txt
```
Quick partial ls of very big folder (-U suppresses sorting):
```
ls -U | head -40
```
Shuffle lines in a text file:
```
sort -R file.txt > out.txt
```
In fact is sorts by hashes, so it's semi-random and stable.

4 Mar 2021
----
Solution to "Argument list too long" error while trying to mv or cp a lot of files in bash:
instead of mv * target_dir/
```
ls | xargs mv -t target_dir/
```
Replace in file syntax:
```
sed -i 's/old_dir/new_dir\/subdir_after_escaping_slash/' test.txt
```
Delete all lines containing string 'my_text' inline:
```
sed -i '/my_text/d' filename.txt 
````
Prepend dir name to each line (replace ^ which is beginning of a line):
```
sed -i.bak 's/^/dir_name\//' infile.txt
```
-i.bak flag makes sed work inplace, but creates backup file with .bak extension.

Quitting hanging ssh session:
ctrl + d, if it doesn't work:
enter ~ .

Jupyter: stopping execution programmatically:
```
class StopExecution(Exception):
    def _render_traceback_(self):
        pass
# ....
raise StopExecution
```
1 Mar 2021
----
Darknet/YOLO_v4 - tips:
It's surprisingly hard to find the actual architecture of YOLO models; and there is no Tensorboard to browse through the computation graph.
It appears that https://netron.app/ and https://github.com/lutzroeder/netron support Darknet's .cfg files and return beautiful, interactive graphs.
These graphs help to understand how the 3 [yolo] layers (2 for tiny-yolo) are attached (deep supervision of the model).

Problems:
1) The relation between batch/minibatch/subdivision/epoch/iteration/darknet binary outputs/steps/brun_in values/XXXX.weights is counterintuitive to me. 
Asked questions on stack overflow, run some experiments; waiting for results.
2) I would greatly appreciate if darknet returned current learning rate alongside with other metrics - perhaps I will fork and rebuild it.
3) I still don't understand anchors/mask parameters per [yolo] layer instance. Why only a small portion of anchors is used at all?

18 Feb 2021
----
My struggle to a find short but accurate description of the YOLO object detection network:

First, we had sliding windows + classification.
Then R-CNN use unsupervised method (selective search) to propose bounding boxes.
Fast R-CNN proposed bounding boxes after features extraction with the backbone network.
Faster R-CNN learned a separate neural network to learn region proposals. The classification network also predicted offsets for BBs.
YOLO introduced grid, and for each grid cell m bounding boxes (aka. anchors) are generated. One network predicts class, offset, width/height adjustment.

16 Feb 2021
----
Setting DNS records is always painful - tiny details matter, tutorials are often wrong, changes take undefined time to propagate.
Github pages + GoDaddy:
First had to disable redirection (which was hidden deep). Than set:
```
a 	@ 	185.199.108.153
a 	@ 	185.199.109.153
a 	@ 	185.199.110.153
a 	@ 	185.199.111.153
cname 	www 	@
cname 	www.drapej.com 	@
```
A records MUST have '@' as 'name'.
_domainconnect can be deleted or kept - doesn't matter
TTS - kept default values.

The repo has to be named huxwell.github.io, (huxwell = my username), Settings->Custom Domain name MUST be www.drapej.com not drapej.com. 
CNAME file in repo root: www.drapej.com (NOT drapej.com)

15 Feb 2021
----
Git deleting branches:
```
git push origin --delete branch_name
git branch -D branch_name
```
Jupyter logging model training with Darknet (%%capture will suppress live output, so it's not an option):
import datetime
file_prefix = datetime.datetime.now().strftime("%Y_%m_%d-%H_%M_%S")
!./darknet detector params 2>&1 | tee ../train_logs/{file_prefix}.txt

12 Feb 2021
----
Neural Networks/ML in general:
How to label border cases?
Is 15% of a cat's tail a cat?
Is very blurred image of cat a cat?
Should we use soft targets for neural networks?
(0.8 0.2) (cat, not cat) ?
Is such regression easier or harder to learn?

Asked a question on ai.stackexchange: 
https://ai.stackexchange.com/questions/26175/how-to-treat-label-and-process-edge-case-inputs-in-machine-learning
Received some long replies, but didn't learn anything new from them :(
I need to find some papers that directly compare different approaches in quantitative way.

11 Feb  2021
----
AWS EC2:
If you stop VMs when you don't use them and then restart, the address will change each time which is super annoying with ssh.
Go to console.aws.amazon.com -> type Elastic IPs in search -> choose ec2 feature (not VPC feature) -> Allocate Elastic IP address (use all default options)
Checkbox newly created IP -> Actions -> Associate Elastic IP -> choose your instance from the list. 
Now you can always scp/sftp/ssh to newly created ip instead of always changing ec2-12-345-678-901.compute-2.amazonaws.com .
According to aws.amazon.com/ec2/pricing/on-demand/ it costs around 12 cents / day if the instance is stopped AFAIU.

10 Feb 2021
----
AWS EC2:
Extend disc space (EBS):
Instances: checkbox target VM, Storage, click volume id, actions, modify volume. 
SSH to machine, df -hT, lsblk to get more info.
sudo growpart /dev/xvda 1 (counterintuitive, because lsblk and aws docs suggest 'xvda1 1')
Should be enough, but df and lsbik show  different values, so I also used:
sudo resize2fs /dev/xvda1

No reboot required!

3 Feb  2021
----
Jupyter shortcuts: Esc to leve editing mode, arrows to jump between cells
```
DD - delete cell
L - show line numbers
Enter - back to editing
H - show shortcuts help
```
Run persistently (more reliable than nohup to me):
jupyter notebook --no-browser --port 8080 &> /dev/null & disown
Connect with forwarding:
```
ssh -L 8888:localhost:8888 some_short_alias
```
3 Feb  2021
----
Github pages: often an old versions of the files are being served. 
https://github.com/user/repo/deployments
( https://github.com/huxwell/huxwell.github.io/deployments for me ) is a way to check if your commits have already been deployed.

2 Feb  2021
----
With ssh (especially for Amazon EC2) it's very usefull to do alias in ~/.ssh/config, i.e:

```
Host some_short_alias
  HostName ec2-12-345-678-901.compute-2.amazonaws.com
  IdentityFile ~/.ssh/some_key_path.pem
  User my_username
```
And than always do ssh a_short_name instead of ssh -i "~/.ssh/some_key_path.pem" ec2-12-345-678-901.compute-2.amazonaws.com
Also, Visual Studio Code adds a record there automatically when you use Remote Explorer-> SSH Targets-> +
Open files explorer and type sftp://a_short_name to user gui ftp file explorer.
Rsync commands sometimes get very messy, so this alias helps a bit:
rsync -av --dry-run /home/local_user/dir/ a_short_name:remote_dir_name


1 Feb 2021
---- 
Hello World! (Or rather Hello Self!)

31 Jan 2021
---- 
