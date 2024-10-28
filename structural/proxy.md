## Proxy


The Proxy pattern introduces an intermediary class (the Proxy) that acts as a surrogate or placeholder for another object (the RealSubject). The Proxy controls access to the RealSubject, potentially adding an extra layer of functionality before or after delegating operations to it. This pattern is useful in various scenarios:

-   **Lazy initialization:** Deferring the creation of the RealSubject until it's actually needed, improving performance.
-   **Access control:** Restricting access to the RealSubject based on permissions or other criteria.
-   **Caching:** Storing frequently accessed data from the RealSubject to avoid redundant operations.
-   **Security:** Mediating access to sensitive resources provided by the RealSubject.
-   **Remote communication:** Handling communication with a remote object as if it were local.


~~~
#include <iostream>
#include <string>

class File {
public:
    virtual ~File() = default;
    virtual std::string getContent() const = 0;
};

class FileDownloader : public File {
public:
    FileDownloader(const std::string& url) : url_(url), content_("") {}

    std::string getContent() const override {
        if (content_.empty()) {
            // Download the file (simulate with a delay)
            std::cout << "Downloading file from " << url_ << "..." << std::endl;
            content_ = "Downloaded content";
        }
        return content_;
    }

private:
    std::string url_;
    mutable std::string content_; // mutable to allow modification within const method
};

class FileDownloaderProxy : public File {
public:
    FileDownloaderProxy(const std::string& url) : downloader_(url) {}

    std::string getContent() const override {
        return downloader_.getContent();
    }

private:
    FileDownloader downloader_;
};

int main() {
    File* file = new FileDownloaderProxy("https://example.com/file.txt");

    // Use the file without immediate download (lazy initialization)
    std::cout << "File not yet downloaded." << std::endl;

    std::string content = file->getContent();
    std::cout << "Downloaded content: " << content << std::endl;

    delete file;
    return 0;
}

~~~