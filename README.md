# Different Printing Targets


http://3dbenchy.com/


# Export from Julia
```julia
    jldsave("PrintingTargets/3D_benchy_550.jld2"; target=target3)
```

Compress with: `tar -czvf 3DBenchy_550.tar.gz 3D_benchy_550.jld2`

```
julia> using SHA, Tar, Inflate

julia> filename = "3DBenchy_550.tar.gz"
"3DBenchy_550.tar.gz"

julia> println("sha256: ", bytes2hex(open(sha256, filename)))
sha256: 6db2ee1c403c3157b8ac39ccc741f440a9f7aceeb737ede6836c3409e1c9b54a

julia> println("git-tree-sha1: ", Tar.tree_hash(IOBuffer(inflate_gzip(filename))))
git-tree-sha1: d70ff565efc152203bb9d4570cb91659b87a3b4f
```


```
# ╔═╡ d708c3a1-6e59-4001-a93a-69442466a9d5
function load_benchy(sz, sz_file, path)
	target = zeros(Float32, sz)
	#@show size(load(joinpath(path, string("slice_", string(1, pad=3) ,".png"))))

	for i in 0:sz_file[1]-1
		target[:, :, 40 + i+1] .= select_region(Gray.(load(joinpath(path, string("boat_", string(i, pad=3) ,".png")))), new_size=(sz))
	end

	#target2 = zeros(Float32, sz)
	#WaveOpticsPropagation.set_center!(target2, target
	target2 = select_region(target, new_size=sz)
	target2 = permutedims(target2, (3,1,2))[end:-1:begin, :, :]
	return togoc(target2)
end

# ╔═╡ 70375fb2-a3f9-482d-b637-7b5867b1e2d0
target3 = permutedims(load_benchy((550, 550, 550), (480, 480, 480), "/home/felix/Downloads/benchy/files/output_480/"), (1,2,3));

```
