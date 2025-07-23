# Harness_Patch

git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
git checkout 091aaf6f31f3e87e81135a5d204c30707711d94a

wget https://github.com/shawn9977/Harness_Patch/blob/main/0001-My-custom-modifications-for-patch.patch
git apply 0001-My-custom-modifications-for-patch.patch
