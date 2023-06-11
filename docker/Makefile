all: setup frontend dist

setup:

# The frontend can only be build using AMD64 (node dependency missing on ARM)
frontend: setup
	docker buildx build -f "$(shell pwd)/Dockerfile-frontend" --output=type=local,dest="$(shell pwd)/build/frontend_output/" --platform linux/amd64 .
	mv "$(shell pwd)/build/frontend_output/home/" "$(shell pwd)/build/filestash_w_frontend/"
	rm -fdr "$(shell pwd)/build/frontend_output/"

./build/filestash_w_frontend/: frontend

# Now, with the frontend ready, the platform dependent image can be build
dist: ./build/filestash_w_frontend/
	docker buildx build -f "$(shell pwd)/Dockerfile-dist" --tag machines/filestash:latest --platform linux/amd64,linux/arm,linux/arm64/v8 .

clean:
	rm -fdr build

.PHONY: all setup frontend dist push clean
