# Obsidian全栈式多媒体文件管理系统升级方案

## 🎯 扩展需求分析

### 新增文件类型管理
- **图片文件**: PNG, JPG, GIF, SVG, WebP等
- **Office文档**: Word, Excel, PowerPoint, PDF
- **代码项目**: Java项目, 配置文件, 脚本
- **多媒体**: 音频, 视频, 3D模型
- **压缩文件**: ZIP, RAR, 7Z等

### 核心挑战
1. **存储策略**: Git对大文件和二进制文件的限制
2. **版本控制**: 如何有效管理二进制文件版本
3. **预览功能**: 各种文件格式的在线预览
4. **搜索索引**: 非文本文件的内容搜索
5. **同步效率**: 大文件的增量同步

---

## 🏗️ 升级后系统架构

```mermaid
graph TB
    subgraph "客户端层"
        A[Obsidian桌面端] --> B[扩展插件系统]
        C[移动端Web] --> D[文件管理界面]
    end
    
    subgraph "服务层"
        E[文件服务API] --> F[预览服务]
        E --> G[转换服务]
        E --> H[索引服务]
    end
    
    subgraph "存储层"
        I[Git LFS] --> J[大文件存储]
        K[对象存储] --> L[图片/文档]
        M[文档数据库] --> N[文件元数据]
        O[全文搜索] --> P[内容索引]
    end
    
    subgraph "智能处理层"
        Q[OCR识别] --> R[图片文字提取]
        S[文档解析] --> T[Office内容提取]
        U[代码分析] --> V[项目结构分析]
    end
    
    B --> E
    D --> E
    E --> I
    E --> K
    E --> M
    E --> Q
    E --> S
    E --> U
    
    style A fill:#e1f5fe
    style E fill:#fff3e0
    style I fill:#e8f5e8
    style Q fill:#f3e5f5
```

---

## 💾 文件存储策略设计

### 1️⃣ 混合存储架构

#### Git + Git LFS + 对象存储
```bash
# 文件存储分层策略
/vault-root/
├── markdown/              # Git直接管理
│   ├── notes/
│   └── templates/
├── assets-small/          # Git管理的小文件 (<1MB)
│   ├── icons/
│   └── thumbnails/
├── assets-large/          # Git LFS管理 (1MB-100MB)
│   ├── documents/
│   ├── images/
│   └── code-projects/
└── assets-huge/           # 对象存储 (>100MB)
    ├── videos/
    ├── datasets/
    └── vm-images/
```

#### 存储配置
```yaml
# storage-config.yml
storage:
  git_direct:
    max_size: "1MB"
    types: ["md", "txt", "json", "yml", "small-images"]
    
  git_lfs:
    max_size: "100MB"
    types: ["pdf", "docx", "xlsx", "pptx", "large-images", "audio"]
    patterns:
      - "*.pdf"
      - "*.doc*"
      - "*.xls*"
      - "*.ppt*"
      - "*.png"
      - "*.jpg"
      - "*.mp3"
    
  object_storage:
    provider: "minio" # 或 AWS S3, 腾讯云COS等
    bucket: "obsidian-vault-large"
    types: ["video", "large-datasets", "vm-images"]
    patterns:
      - "*.mp4"
      - "*.avi"
      - "*.iso"
      - "*.zip" # >100MB
```

### 2️⃣ 文件组织策略

#### 智能文件分类
```javascript
// file-classifier.js
class FileClassifier {
  constructor() {
    this.rules = {
      images: {
        extensions: ['.png', '.jpg', '.jpeg', '.gif', '.svg', '.webp'],
        path: 'assets/images',
        processing: ['thumbnail', 'ocr', 'metadata']
      },
      documents: {
        extensions: ['.pdf', '.doc', '.docx', '.xls', '.xlsx', '.ppt', '.pptx'],
        path: 'assets/documents',
        processing: ['preview', 'text-extraction', 'index']
      },
      code: {
        extensions: ['.java', '.js', '.py', '.cpp', '.h'],
        path: 'assets/code',
        processing: ['syntax-highlight', 'structure-analysis']
      },
      projects: {
        patterns: ['pom.xml', 'package.json', 'requirements.txt'],
        path: 'assets/projects',
        processing: ['dependency-analysis', 'build-detection']
      },
      config: {
        extensions: ['.ini', '.conf', '.properties', '.yml', '.yaml'],
        path: 'assets/config',
        processing: ['syntax-validation', 'template-matching']
      }
    };
  }
  
  classify(filePath) {
    const ext = path.extname(filePath).toLowerCase();
    const basename = path.basename(filePath);
    
    // 检查项目文件
    for (const [type, config] of Object.entries(this.rules)) {
      if (config.patterns?.includes(basename)) {
        return { type, config, reason: 'pattern-match' };
      }
      if (config.extensions?.includes(ext)) {
        return { type, config, reason: 'extension-match' };
      }
    }
    
    return { type: 'misc', config: this.rules.misc };
  }
}
```

---

## 🖼️ 文件预览系统

### 1️⃣ 多格式预览引擎

#### 图片预览与处理
```javascript
// image-processor.js
class ImageProcessor {
  async processImage(imagePath) {
    const results = await Promise.all([
      this.generateThumbnail(imagePath),
      this.extractMetadata(imagePath),
      this.performOCR(imagePath),
      this.detectObjects(imagePath)
    ]);
    
    return {
      thumbnail: results[0],
      metadata: results[1],
      textContent: results[2],
      objects: results[3]
    };
  }
  
  async generateThumbnail(imagePath) {
    const sharp = require('sharp');
    
    const thumbnailPath = imagePath.replace(/\.[^.]+$/, '_thumb.webp');
    
    await sharp(imagePath)
      .resize(300, 300, { fit: 'inside', withoutEnlargement: true })
      .webp({ quality: 80 })
      .toFile(thumbnailPath);
    
    return thumbnailPath;
  }
  
  async performOCR(imagePath) {
    const tesseract = require('tesseract.js');
    
    const { data: { text } } = await tesseract.recognize(
      imagePath,
      'chi_sim+eng', // 支持中英文
      {
        logger: m => console.log(m)
      }
    );
    
    return text.trim();
  }
}
```

#### Office文档预览
```javascript
// office-processor.js
class OfficeProcessor {
  async processDocument(docPath) {
    const ext = path.extname(docPath).toLowerCase();
    
    switch (ext) {
      case '.pdf':
        return this.processPDF(docPath);
      case '.docx':
        return this.processWord(docPath);
      case '.xlsx':
        return this.processExcel(docPath);
      case '.pptx':
        return this.processPowerPoint(docPath);
      default:
        throw new Error(`Unsupported document type: ${ext}`);
    }
  }
  
  async processPDF(pdfPath) {
    const pdfParse = require('pdf-parse');
    const fs = require('fs');
    
    const dataBuffer = fs.readFileSync(pdfPath);
    const data = await pdfParse(dataBuffer);
    
    // 生成PDF预览图
    const preview = await this.generatePDFPreview(pdfPath);
    
    return {
      text: data.text,
      pages: data.numpages,
      info: data.info,
      preview: preview,
      searchIndex: this.createSearchIndex(data.text)
    };
  }
  
  async processWord(docPath) {
    const mammoth = require('mammoth');
    
    const result = await mammoth.extractRawText({ path: docPath });
    const htmlResult = await mammoth.convertToHtml({ path: docPath });
    
    return {
      text: result.value,
      html: htmlResult.value,
      messages: result.messages,
      wordCount: result.value.split(/\s+/).length
    };
  }
  
  async processExcel(xlsxPath) {
    const XLSX = require('xlsx');
    
    const workbook = XLSX.readFile(xlsxPath);
    const sheets = {};
    
    workbook.SheetNames.forEach(sheetName => {
      const worksheet = workbook.Sheets[sheetName];
      sheets[sheetName] = {
        json: XLSX.utils.sheet_to_json(worksheet),
        csv: XLSX.utils.sheet_to_csv(worksheet),
        html: XLSX.utils.sheet_to_html(worksheet)
      };
    });
    
    return {
      sheets: sheets,
      sheetNames: workbook.SheetNames,
      summary: this.generateExcelSummary(sheets)
    };
  }
}
```

#### Java项目分析
```javascript
// java-analyzer.js
class JavaProjectAnalyzer {
  async analyzeProject(projectPath) {
    const analysis = {
      structure: await this.analyzeStructure(projectPath),
      dependencies: await this.analyzeDependencies(projectPath),
      classes: await this.analyzeClasses(projectPath),
      tests: await this.analyzeTests(projectPath),
      config: await this.analyzeConfig(projectPath)
    };
    
    return analysis;
  }
  
  async analyzeStructure(projectPath) {
    const glob = require('glob');
    
    const javaFiles = glob.sync('**/*.java', { cwd: projectPath });
    const packages = [...new Set(
      javaFiles.map(file => path.dirname(file).replace(/\//g, '.'))
    )];
    
    return {
      totalFiles: javaFiles.length,
      packages: packages,
      structure: this.buildPackageTree(javaFiles)
    };
  }
  
  async analyzeDependencies(projectPath) {
    const pomPath = path.join(projectPath, 'pom.xml');
    const gradlePath = path.join(projectPath, 'build.gradle');
    
    if (fs.existsSync(pomPath)) {
      return this.analyzeMavenDependencies(pomPath);
    } else if (fs.existsSync(gradlePath)) {
      return this.analyzeGradleDependencies(gradlePath);
    }
    
    return { type: 'unknown', dependencies: [] };
  }
  
  async analyzeClasses(projectPath) {
    const javaFiles = glob.sync('**/*.java', { cwd: projectPath });
    const classes = [];
    
    for (const file of javaFiles) {
      const content = fs.readFileSync(path.join(projectPath, file), 'utf8');
      const classInfo = this.parseJavaClass(content);
      classes.push({ file, ...classInfo });
    }
    
    return classes;
  }
  
  parseJavaClass(content) {
    // 简化的Java类解析
    const classMatch = content.match(/class\s+(\w+)/);
    const methodMatches = content.match(/\w+\s+\w+\s*\([^)]*\)\s*\{/g) || [];
    const fieldMatches = content.match(/private|public|protected\s+\w+\s+\w+;/g) || [];
    
    return {
      className: classMatch ? classMatch[1] : 'Unknown',
      methods: methodMatches.length,
      fields: fieldMatches.length,
      lines: content.split('\n').length
    };
  }
}
```

### 2️⃣ Web端预览界面

#### React组件设计
```jsx
// FilePreview.jsx
import React, { useState, useEffect } from 'react';
import { Document, Page } from 'react-pdf'; // PDF预览
import { Controlled as CodeMirror } from 'react-codemirror2'; // 代码预览

const FilePreview = ({ file, onClose }) => {
  const [previewData, setPreviewData] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    loadPreviewData();
  }, [file]);
  
  const loadPreviewData = async () => {
    try {
      const response = await fetch(`/api/files/preview/${file.id}`);
      const data = await response.json();
      setPreviewData(data);
    } catch (error) {
      console.error('Preview loading failed:', error);
    } finally {
      setLoading(false);
    }
  };
  
  const renderPreview = () => {
    if (!previewData) return <div>无法预览此文件</div>;
    
    switch (file.type) {
      case 'image':
        return <ImagePreview data={previewData} />;
      case 'pdf':
        return <PDFPreview data={previewData} />;
      case 'office':
        return <OfficePreview data={previewData} />;
      case 'code':
        return <CodePreview data={previewData} />;
      case 'java-project':
        return <JavaProjectPreview data={previewData} />;
      default:
        return <GenericPreview data={previewData} />;
    }
  };
  
  return (
    <div className="file-preview-modal">
      <div className="preview-header">
        <h3>{file.name}</h3>
        <button onClick={onClose}>×</button>
      </div>
      <div className="preview-content">
        {loading ? <div>加载中...</div> : renderPreview()}
      </div>
      <div className="preview-metadata">
        <FileMetadata file={file} data={previewData} />
      </div>
    </div>
  );
};

// 图片预览组件
const ImagePreview = ({ data }) => (
  <div className="image-preview">
    <img src={data.url} alt="preview" className="max-w-full max-h-96" />
    {data.ocrText && (
      <div className="ocr-text mt-4 p-4 bg-gray-100 rounded">
        <h4>识别的文字：</h4>
        <p>{data.ocrText}</p>
      </div>
    )}
    <div className="image-metadata mt-4">
      <p>尺寸: {data.width} × {data.height}</p>
      <p>大小: {data.size}</p>
      <p>格式: {data.format}</p>
    </div>
  </div>
);

// PDF预览组件
const PDFPreview = ({ data }) => {
  const [pageNumber, setPageNumber] = useState(1);
  
  return (
    <div className="pdf-preview">
      <div className="pdf-controls">
        <button onClick={() => setPageNumber(Math.max(1, pageNumber - 1))}>
          上一页
        </button>
        <span>{pageNumber} / {data.totalPages}</span>
        <button onClick={() => setPageNumber(Math.min(data.totalPages, pageNumber + 1))}>
          下一页
        </button>
      </div>
      <Document file={data.url}>
        <Page pageNumber={pageNumber} />
      </Document>
      {data.extractedText && (
        <div className="pdf-text mt-4">
          <h4>文档内容：</h4>
          <pre className="whitespace-pre-wrap">{data.extractedText}</pre>
        </div>
      )}
    </div>
  );
};

// Java项目预览组件
const JavaProjectPreview = ({ data }) => (
  <div className="java-project-preview">
    <div className="project-summary grid grid-cols-2 gap-4 mb-6">
      <div className="bg-blue-100 p-4 rounded">
        <h4>项目结构</h4>
        <p>文件数: {data.structure.totalFiles}</p>
        <p>包数: {data.structure.packages.length}</p>
      </div>
      <div className="bg-green-100 p-4 rounded">
        <h4>依赖管理</h4>
        <p>类型: {data.dependencies.type}</p>
        <p>依赖数: {data.dependencies.count}</p>
      </div>
    </div>
    
    <div className="project-details">
      <div className="mb-4">
        <h4>主要类</h4>
        <ul className="list-disc list-inside">
          {data.classes.slice(0, 10).map(cls => (
            <li key={cls.file}>
              {cls.className} - {cls.methods}个方法, {cls.fields}个字段
            </li>
          ))}
        </ul>
      </div>
      
      <div className="mb-4">
        <h4>依赖列表</h4>
        <div className="dependencies-list max-h-40 overflow-y-auto">
          {data.dependencies.list?.map(dep => (
            <div key={dep.name} className="p-2 border-b">
              {dep.name} - {dep.version}
            </div>
          ))}
        </div>
      </div>
    </div>
  </div>
);
```

---

## 🔄 版本管理与同步策略

### 1️⃣ Git LFS配置

#### .gitattributes配置
```gitattributes
# 图片文件使用LFS
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.gif filter=lfs diff=lfs merge=lfs -text
*.webp filter=lfs diff=lfs merge=lfs -text
*.svg filter=lfs diff=lfs merge=lfs -text

# Office文档使用LFS
*.pdf filter=lfs diff=lfs merge=lfs -text
*.doc filter=lfs diff=lfs merge=lfs -text
*.docx filter=lfs diff=lfs merge=lfs -text
*.xls filter=lfs diff=lfs merge=lfs -text
*.xlsx filter=lfs diff=lfs merge=lfs -text
*.ppt filter=lfs diff=lfs merge=lfs -text
*.pptx filter=lfs diff=lfs merge=lfs -text

# 压缩文件使用LFS
*.zip filter=lfs diff=lfs merge=lfs -text
*.rar filter=lfs diff=lfs merge=lfs -text
*.7z filter=lfs diff=lfs merge=lfs -text

# Java编译文件忽略
*.class
*.jar filter=lfs diff=lfs merge=lfs -text
target/
.idea/
*.iml

# 配置文件
*.properties
*.yml
*.yaml
*.xml
*.json
```

### 2️⃣ 智能同步策略

#### 同步管理器
```javascript
// sync-manager.js
class SyncManager {
  constructor() {
    this.syncQueue = [];
    this.conflictResolver = new ConflictResolver();
    this.bandwidth = new BandwidthManager();
  }
  
  async syncFile(file) {
    const strategy = this.decideSyncStrategy(file);
    
    switch (strategy) {
      case 'git-direct':
        return this.syncViaGit(file);
      case 'git-lfs':
        return this.syncViaLFS(file);
      case 'object-storage':
        return this.syncViaObjectStorage(file);
      case 'skip':
        return this.skipSync(file);
    }
  }
  
  decideSyncStrategy(file) {
    // 根据文件大小和类型决定同步策略
    if (file.size < 1024 * 1024) { // < 1MB
      return 'git-direct';
    } else if (file.size < 100 * 1024 * 1024) { // < 100MB
      return 'git-lfs';
    } else {
      return 'object-storage';
    }
  }
  
  async syncViaObjectStorage(file) {
    const minio = require('minio');
    
    const minioClient = new minio.Client({
      endPoint: process.env.MINIO_ENDPOINT,
      port: parseInt(process.env.MINIO_PORT),
      useSSL: process.env.MINIO_SSL === 'true',
      accessKey: process.env.MINIO_ACCESS_KEY,
      secretKey: process.env.MINIO_SECRET_KEY
    });
    
    // 上传文件到对象存储
    const objectName = `${file.hash}/${file.name}`;
    await minioClient.fPutObject('obsidian-vault', objectName, file.path);
    
    // 在Git中保存引用文件
    const reference = {
      name: file.name,
      hash: file.hash,
      size: file.size,
      type: file.type,
      storage: 'minio',
      objectName: objectName,
      uploadTime: new Date().toISOString()
    };
    
    const refPath = file.path + '.ref';
    fs.writeFileSync(refPath, JSON.stringify(reference, null, 2));
    
    return { success: true, storage: 'object-storage', reference: refPath };
  }
}
```

### 3️⃣ 冲突解决机制

#### 智能冲突解决
```javascript
// conflict-resolver.js
class ConflictResolver {
  async resolveConflict(localFile, remoteFile) {
    const conflictType = this.identifyConflictType(localFile, remoteFile);
    
    switch (conflictType) {
      case 'binary-different':
        return this.resolveBinaryConflict(localFile, remoteFile);
      case 'text-mergeable':
        return this.resolveTextConflict(localFile, remoteFile);
      case 'config-conflict':
        return this.resolveConfigConflict(localFile, remoteFile);
      case 'version-mismatch':
        return this.resolveVersionConflict(localFile, remoteFile);
    }
  }
  
  async resolveBinaryConflict(localFile, remoteFile) {
    // 对于二进制文件，提供选择界面
    const choice = await this.promptUserChoice({
      local: {
        file: localFile,
        preview: await this.generatePreview(localFile),
        metadata: await this.extractMetadata(localFile)
      },
      remote: {
        file: remoteFile,
        preview: await this.generatePreview(remoteFile),
        metadata: await this.extractMetadata(remoteFile)
      }
    });
    
    switch (choice) {
      case 'keep-local':
        return { resolution: 'local', action: 'keep' };
      case 'keep-remote':
        return { resolution: 'remote', action: 'overwrite' };
      case 'keep-both':
        return { resolution: 'both', action: 'rename' };
      case 'merge-content':
        return this.attemptContentMerge(localFile, remoteFile);
    }
  }
}
```

---

## 🔍 搜索与索引系统

### 1️⃣ 全文搜索引擎

#### Elasticsearch配置
```yaml
# docker-compose.search.yml
version: '3.8'
services:
  elasticsearch:
    image: elasticsearch:8.0.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    ports:
      - "9200:9200"
    volumes:
      - es_data:/usr/share/elasticsearch/data
  
  kibana:
    image: kibana:8.0.0
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

volumes:
  es_data:
```

#### 文档索引器
```javascript
// document-indexer.js
class DocumentIndexer {
  constructor() {
    this.elasticsearch = require('@elastic/elasticsearch');
    this.client = new this.elasticsearch.Client({
      node: process.env.ELASTICSEARCH_URL || 'http://localhost:9200'
    });
  }
  
  async indexFile(file) {
    const content = await this.extractSearchableContent(file);
    
    const document = {
      id: file.id,
      name: file.name,
      path: file.path,
      type: file.type,
      size: file.size,
      created: file.created,
      modified: file.modified,
      content: content.text,
      metadata: content.metadata,
      tags: file.tags || [],
      searchableContent: [
        file.name,
        content.text,
        content.metadata?.title,
        content.metadata?.author,
        ...(file.tags || [])
      ].filter(Boolean).join(' ')
    };
    
    await this.client.index({
      index: 'obsidian-files',
      id: file.id,
      body: document
    });
  }
  
  async extractSearchableContent(file) {
    switch (file.type) {
      case 'markdown':
        return this.extractMarkdownContent(file);
      case 'pdf':
        return this.extractPDFContent(file);
      case 'office':
        return this.extractOfficeContent(file);
      case 'image':
        return this.extractImageContent(file);
      case 'code':
        return this.extractCodeContent(file);
      default:
        return { text: '', metadata: {} };
    }
  }
  
  async search(query, options = {}) {
    const searchBody = {
      query: {
        multi_match: {
          query: query,
          fields: [
            'name^3',           // 文件名权重最高
            'content^2',        // 内容权重中等
            'searchableContent', // 综合内容权重正常
            'metadata.title^2',
            'tags^1.5'
          ],
          fuzziness: 'AUTO'
        }
      },
      highlight: {
        fields: {
          content: {},
          name: {}
        }
      },
      size: options.size || 20,
      from: options.from || 0
    };
    
    // 添加过滤器
    if (options.type) {
      searchBody.query = {
        bool: {
          must: searchBody.query,
          filter: { term: { type: options.type } }
        }
      };
    }
    
    const response = await this.client.search({
      index: 'obsidian-files',
      body: searchBody
    });
    
    return {
      total: response.body.hits.total.value,
      results: response.body.hits.hits.map(hit => ({
        ...hit._source,
        score: hit._score,
        highlights: hit.highlight
      }))
    };
  }
}
```

---

## 📱 移动端升级方案

### 1️⃣ 文件管理界面

#### React Native组件
```jsx
// FileManager.jsx
import React, { useState, useEffect } from 'react';
import { View, FlatList, TouchableOpacity, Text, Image } from 'react-native';
import DocumentPicker from 'react-native-document-picker';

const FileManager = () => {
  const [files, setFiles] = useState([]);
  const [selectedFile, setSelectedFile] = useState(null);
  const [filter, setFilter] = useState('all');
  
  const renderFileItem = ({ item }) => (
    <TouchableOpacity 
      style={styles.fileItem}
      onPress={() => openFile(item)}
    >
      <View style={styles.fileIcon}>
        {getFileIcon(item.type)}
      </View>
      <View style={styles.fileInfo}>
        <Text style={styles.fileName}>{item.name}</Text>
        <Text style={styles.fileDetails}>
          {formatFileSize(item.size)} • {formatDate(item.modified)}
        </Text>
        {item.preview && (
          <Text style={styles.filePreview} numberOfLines={2}>
            {item.preview}
          </Text>
        )}
      </View>
    </TouchableOpacity>
  );
  
  const getFileIcon = (type) => {
    const icons = {
      'pdf': '📄',
      'image': '🖼️',
      'code': '📝',
      'java-project': '☕',
      'office': '📊',
      'config': '⚙️'
    };
    return <Text style={styles.icon}>{icons[type] || '📁'}</Text>;
  };
  
  return (
    <View style={styles.container}>
      <FileFilterBar filter={filter} onFilterChange={setFilter} />
      <FlatList
        data={files.filter(file => filter === 'all' || file.type === filter)}
        renderItem={renderFileItem}
        keyExtractor={item => item.id}
      />
      {selectedFile && (
        <FilePreviewModal 
          file={selectedFile} 
          onClose={() => setSelectedFile(null)}
        />
      )}
    </View>
  );
};
```

### 2️⃣ 离线文件缓存

#### 缓存管理策略
```javascript
// mobile-cache-manager.js
class MobileCacheManager {
  constructor() {
    this.cacheStorage = new Map();
    this.maxCacheSize = 500 * 1024 * 1024; // 500MB
    this.cacheDir = '/cache/obsidian-files/';
  }
  
  async cacheFile(file) {
    // 检查缓存空间
    if (await this.getCacheSize() + file.size > this.maxCacheSize) {
      await this.evictOldFiles();
    }
    
    const cacheKey = this.generateCacheKey(file);
    const cachePath = path.join(this.cacheDir, cacheKey);
    
    // 下载并缓存文件
    await this.downloadFile(file.url, cachePath);
    
    // 生成预览
    const preview = await this.generateMobilePreview(file, cachePath);
    
    // 保存缓存信息
    this.cacheStorage.set(cacheKey, {
      file: file,
      cachePath: cachePath,
      preview: preview,
      cachedAt: new Date(),
      accessCount: 0,
      lastAccess: new Date()
    });
    
    return { cachePath, preview };
  }
  
  async generateMobilePreview(file, filePath) {
    switch (file.type) {
      case 'image':
        return this.generateImageThumbnail(filePath);
      case 'pdf':
        return this.generatePDFThumbnail(filePath);
      case 'office':
        return this.generateOfficeThumbnail(filePath);
      default:
        return null;
    }
  }
  
  async generateImageThumbnail(imagePath) {
    const { Image } = require('react-native');
    
    // 生成适合移动端的缩略图
    const thumbnail = await Image.getSize(imagePath);
    const maxDimension = 200;
    const scale = Math.min(maxDimension / thumbnail.width, maxDimension / thumbnail.height);
    
    return {
      width: thumbnail.width * scale,
      height: thumbnail.height * scale,
      path: imagePath
    };
  }
}
```

---

## 🚀 部署与配置升级

### 1️⃣ Docker Compose完整配置

```yaml
# docker-compose.full.yml
version: '3.8'

services:
  # 主应用
  obsidian-web:
    build: 
      context: .
      dockerfile: Dockerfile.web
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://postgres:password@postgres:5432/obsidian
      - ELASTICSEARCH_URL=http://elasticsearch:9200
      - MINIO_ENDPOINT=minio
      - REDIS_URL=redis://redis:6379
    volumes:
      - ./vault:/app/vault
      - ./uploads:/app/uploads
    depends_on:
      - postgres
      - redis
      - elasticsearch
      - minio
  
  # 文件处理服务
  file-processor:
    build:
      context: .
      dockerfile: Dockerfile.processor
    environment:
      - TESSDATA_PREFIX=/usr/share/tesseract-ocr/4.00/tessdata
    volumes:
      - ./vault:/app/vault
      - ./temp:/app/temp
    depends_on:
      - redis
  
  # 数据库
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_DB=obsidian
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  # 缓存
  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data
  
  # 搜索引擎
  elasticsearch:
    image: elasticsearch:8.0.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
    volumes:
      - es_data:/usr/share/elasticsearch/data
  
  # 对象存储
  minio:
    image: minio/minio
    command: server /data --console-address ":9001"
    ports:
      - "9000:9000"
      - "9001:9001"
    environment:
      - MINIO_ACCESS_KEY=minioadmin
      - MINIO_SECRET_KEY=minioadmin
    volumes:
      - minio_data:/data
  
  # 反向代理
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - obsidian-web

volumes:
  postgres_data:
  redis_data:
  es_data:
  minio_data:
```

### 2️⃣ 自动化部署脚本

```bash
#!/bin/bash
# deploy-full-system.sh

echo "🚀 部署Obsidian全栈文件管理系统..."

# 1. 创建必要目录
mkdir -p {vault,uploads,temp,logs,ssl}

# 2. 设置环境变量
cp .env.example .env
echo "请编辑.env文件配置相关参数"

# 3. 初始化Git LFS
cd vault
git lfs install
git lfs track "*.png" "*.jpg" "*.pdf" "*.docx"

# 4. 构建镜像
docker-compose -f docker-compose.full.yml build

# 5. 启动服务
docker-compose -f docker-compose.full.yml up -d

# 6. 等待服务启动
echo "等待服务启动..."
sleep 30

# 7. 初始化数据库
docker-compose exec obsidian-web npm run db:migrate
docker-compose exec obsidian-web npm run db:seed

# 8. 创建Elasticsearch索引
curl -X PUT "localhost:9200/obsidian-files" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "name": {"type": "text", "analyzer": "standard"},
      "content": {"type": "text", "analyzer": "standard"},
      "type": {"type": "keyword"},
      "tags": {"type": "keyword"},
      "created": {"type": "date"},
      "modified": {"type": "date"}
    }
  }
}'

# 9. 设置定时任务
echo "0 2 * * * /app/scripts/backup.sh" | crontab -

echo "✅ 部署完成！"
echo "🌐 Web访问: http://localhost"
echo "📊 Minio控制台: http://localhost:9001"
echo "🔍 Kibana: http://localhost:5601"
```

---

## 💡 使用指南总结

### 🎯 **升级后的完整功能**

1. **全类型文件支持**: 图片、Office、Java项目、配置文件等
2. **智能预览系统**: OCR文字识别、PDF预览、Java项目分析
3. **混合存储策略**: Git + Git LFS + 对象存储
4. **全文搜索引擎**: Elasticsearch支持所有文件类型搜索
5. **移动端优化**: 离线缓存、响应式界面、文件管理

### 🔧 **配置建议**

- **小团队**: 使用Git LFS即可，成本低
- **大团队**: 配置对象存储，扩展性好
- **个人使用**: 简化版本，去掉Elasticsearch
- **企业级**: 全功能版本，加强安全配置

这套升级方案将你的Obsidian系统变成了一个功能完整的**数字资产管理平台**，不仅管理Markdown笔记，还能高效处理所有类型的文件资源！🎯