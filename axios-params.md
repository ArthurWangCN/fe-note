```js
getArticleByKeyword(keywords) {
      let params = {
				keywords: keywords,
				pageIndex: this.articlePageIndex,
				pageSize: this.articlePageSize
			}
      // params = qs.stringify(params, { indices: false });
      axios({
        url: `${window.applicationPath}/map/getArticleByKeyword`,
        method: 'GET',
        params,
        paramsSerializer: params => {
          return qs.stringify(params, { indices: false })
        }
      }).then(({data}) => {
				if (data.success) {
					let arr = data.content.articleList;
					this.articleTotal = data.total;
					this.articleList.push(...arr);
					this.articlePageIndex++;
				} else {
          this.$message.error(data.message);
				}
			}).catch(err => {
          this.$message.error(err);
			}).finally(_ => {

      });
    },
```
