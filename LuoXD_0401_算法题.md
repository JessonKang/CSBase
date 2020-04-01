


```c++
class Solution {
public:
	/* 杨辉三角 */
	vector<vector<int>> generate(int numRows) {
		vector<vector<int>> res;
		if (numRows == 0)
			return res;
		if (numRows == 1)
		{	
			vector<int> v;
			v.push_back(1);
			res.push_back(v);
			return res;
		}
		if (numRows == 2)
		{
			vector<int> v1;
			v1.push_back(1);
			res.push_back(v1);
			vector<int> v2;
			v2.push_back(1);
			v2.push_back(1);
			res.push_back(v2);
			return res;
		}
		else {
			vector<int> v1;
			v1.push_back(1);
			res.push_back(v1);
			vector<int> v2;
			v2.push_back(1);
			v2.push_back(1);
			res.push_back(v2);
			vector<int> v;
			v.push_back(1); 
			v.push_back(1);/* 占个位出来，要不然v[j]无法操作，这样就可以使用同一个v向量了，res.push_back(v) 这是拷贝操作 */
			for (int i = 2; i < numRows; i++)
			{
				for (int j = 1; j < i; j++)
				{
					v[j] = res[i - 1][j - 1] + res[i - 1][j];
				}
				v.push_back(1);
				res.push_back(v);
			}
			return res;
		}
	}
/* 杨辉三角，返回第k(索引)行 */
vector<int> generate2(int rowIndex) {
	vector<int> res(rowIndex +1,1);
	if (rowIndex == 0 || rowIndex == 1)
		return res;
	else {
		for (int i = 2; i <= rowIndex; i++)
		{
			int a = res[0], b = res[1];
			for (int j = 1; j < i; j++)
			{
				res[j] = a + b;
				if (j % 2 == 1)
				{
					a = res[j + 1];
					continue;
				}
				else
					b = res[j + 1];
			}
		}
		return res;
	}
}

/* 验证回文串,给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。 */
bool isPalindrome(string s) {
	int len = s.length();
	if (len == 0)
		return true;
	int i, j;
	for (i = 0, j = len - 1; i <= j;)
	{
		int a = s[i];
		/* 是字符或数字 */
		//if (a >= 48 && a <= 57 || a >= 65 && a <= 90 || a >= 97 && a <= 122)
		if(isalnum(s[i]))
		{
			/*if (a >= 97)
				a -= 32;*/
			a = toupper(s[i]);
			int b = s[j];
			//if (b >= 48 && b <= 57 || b >= 65 && b <= 90 || b >= 97 && b <= 122)
			if (isalnum(s[j]))
			{
				/*if (b >= 97)
					b -= 32;*/
				b = toupper(s[j]);
				if (a != b)
					return false;
				else {
					i++;
					j--;
				}
				continue;
			}
			else {
				j--;
			}
			continue;
		}
		else {
			i++;
		}
	}
	return true;
}
};
```
